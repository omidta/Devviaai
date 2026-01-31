# Application Component Template

**Component ID**: [COMP-APP-XXX]  
**Component Name**: [Component Name]  
**Version**: 1.0  
**Status**: Draft | In Development | Complete

## Metadata

| Field | Value |
|-------|-------|
| **Implements Requirements** | FR-XXX, FR-XXX, NFR-XXX |
| **Task ID** | TASK-XXX |
| **Resource ID** | RES-AZ-XXX |
| **Technology Stack** | [Language/Framework] |
| **Port** | [Port Number] |
| **Dependencies** | COMP-APP-XXX, RES-AZ-XXX |

---

## 1. Component Overview

### Purpose
[Brief description of what this component does]

### Responsibilities
- [Responsibility 1]
- [Responsibility 2]
- [Responsibility 3]

### Interfaces
- **REST API**: [Endpoints provided]
- **Database**: [Database connections]
- **Message Queue**: [Queue subscriptions/publications]
- **External APIs**: [Third-party integrations]

---

## 2. Project Structure

```
[component-name]/
├── src/
│   ├── controllers/           # HTTP request handlers
│   │   ├── auth.controller.js
│   │   └── user.controller.js
│   ├── services/              # Business logic
│   │   ├── auth.service.js
│   │   └── user.service.js
│   ├── repositories/          # Data access layer
│   │   └── user.repository.js
│   ├── models/                # Data models/schemas
│   │   └── user.model.js
│   ├── middleware/            # Express middleware
│   │   ├── auth.middleware.js
│   │   ├── error.middleware.js
│   │   └── logging.middleware.js
│   ├── config/                # Configuration
│   │   ├── database.js
│   │   └── app.js
│   ├── utils/                 # Utilities
│   │   ├── logger.js
│   │   └── validator.js
│   ├── routes/                # Route definitions
│   │   ├── auth.routes.js
│   │   └── user.routes.js
│   └── index.js               # Application entry point
├── tests/
│   ├── unit/                  # Unit tests
│   │   ├── controllers/
│   │   ├── services/
│   │   └── repositories/
│   ├── integration/           # Integration tests
│   │   ├── auth.test.js
│   │   └── user.test.js
│   ├── e2e/                   # End-to-end tests
│   │   └── user-flow.test.js
│   └── fixtures/              # Test data
│       └── users.json
├── docs/
│   ├── API.md                 # API documentation
│   ├── SETUP.md               # Setup guide
│   └── ARCHITECTURE.md        # Component architecture
├── scripts/
│   ├── migrate.sh             # Database migrations
│   └── seed.sh                # Seed data
├── .env.example               # Environment template
├── .gitignore
├── Dockerfile                 # Container definition
├── docker-compose.yml         # Local development
├── package.json               # Dependencies
├── jest.config.js             # Test configuration
├── .eslintrc.js               # Linting rules
└── README.md                  # Component README
```

---

## 3. Implementation Details

### 3.1 Controllers

**Purpose**: Handle HTTP requests, validate input, call services, return responses

**Example**: `src/controllers/auth.controller.js`

```javascript
/**
 * Authentication Controller
 * Implements: FR-001 (Azure AD Authentication)
 * Component: COMP-APP-001
 */

const authService = require('../services/auth.service');
const logger = require('../utils/logger');
const { validateLoginRequest } = require('../utils/validator');

/**
 * Handle user login
 * POST /auth/login
 */
async function login(req, res, next) {
  try {
    // Validate input
    const { error } = validateLoginRequest(req.body);
    if (error) {
      return res.status(400).json({
        error: 'Validation error',
        details: error.details
      });
    }

    // Authenticate user
    const { username, password } = req.body;
    const result = await authService.authenticate(username, password);

    // Log successful authentication
    logger.info('User authenticated', {
      userId: result.user.id,
      timestamp: new Date().toISOString()
    });

    // Return token
    res.status(200).json({
      token: result.token,
      expiresIn: result.expiresIn,
      user: {
        id: result.user.id,
        username: result.user.username,
        roles: result.user.roles
      }
    });
  } catch (error) {
    logger.error('Authentication failed', {
      error: error.message,
      username: req.body.username
    });
    next(error);
  }
}

/**
 * Handle user logout
 * POST /auth/logout
 */
async function logout(req, res, next) {
  try {
    const userId = req.user.id;
    await authService.logout(userId);

    logger.info('User logged out', { userId });

    res.status(200).json({
      message: 'Logged out successfully'
    });
  } catch (error) {
    logger.error('Logout failed', { error: error.message });
    next(error);
  }
}

module.exports = {
  login,
  logout
};
```

### 3.2 Services

**Purpose**: Implement business logic, orchestrate operations

**Example**: `src/services/auth.service.js`

```javascript
/**
 * Authentication Service
 * Implements: FR-001, SEC-AUTH-001
 * Component: COMP-APP-001
 */

const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');
const userRepository = require('../repositories/user.repository');
const redisClient = require('../config/redis');
const config = require('../config/app');
const logger = require('../utils/logger');

class AuthService {
  /**
   * Authenticate user with username and password
   * @param {string} username - User's username
   * @param {string} password - User's password
   * @returns {Object} Authentication result with token
   */
  async authenticate(username, password) {
    // Find user in database
    const user = await userRepository.findByUsername(username);
    
    if (!user) {
      throw new Error('Invalid credentials');
    }

    // Verify password
    const isValidPassword = await bcrypt.compare(password, user.passwordHash);
    
    if (!isValidPassword) {
      throw new Error('Invalid credentials');
    }

    // Check if user is active
    if (!user.isActive) {
      throw new Error('Account is inactive');
    }

    // Generate JWT token
    const token = jwt.sign(
      {
        userId: user.id,
        username: user.username,
        roles: user.roles
      },
      config.jwtSecret,
      {
        expiresIn: config.jwtExpiresIn
      }
    );

    // Store session in Redis (implements NFR-012: session timeout)
    await redisClient.setex(
      `session:${user.id}`,
      config.sessionTimeout,
      JSON.stringify({
        userId: user.id,
        username: user.username,
        loginTime: new Date().toISOString()
      })
    );

    return {
      token,
      expiresIn: config.jwtExpiresIn,
      user: {
        id: user.id,
        username: user.username,
        roles: user.roles
      }
    };
  }

  /**
   * Logout user and invalidate session
   * @param {string} userId - User ID
   */
  async logout(userId) {
    // Remove session from Redis
    await redisClient.del(`session:${userId}`);
    
    logger.info('Session invalidated', { userId });
  }

  /**
   * Verify JWT token
   * @param {string} token - JWT token
   * @returns {Object} Decoded token payload
   */
  async verifyToken(token) {
    try {
      const decoded = jwt.verify(token, config.jwtSecret);
      
      // Check if session exists in Redis
      const session = await redisClient.get(`session:${decoded.userId}`);
      
      if (!session) {
        throw new Error('Session expired');
      }

      return decoded;
    } catch (error) {
      throw new Error('Invalid token');
    }
  }
}

module.exports = new AuthService();
```

### 3.3 Repositories

**Purpose**: Handle data access, abstract database operations

**Example**: `src/repositories/user.repository.js`

```javascript
/**
 * User Repository
 * Data access layer for user entities
 * Component: COMP-APP-002
 */

const { pool } = require('../config/database');
const logger = require('../utils/logger');

class UserRepository {
  /**
   * Find user by username
   * @param {string} username - Username to search for
   * @returns {Object|null} User object or null
   */
  async findByUsername(username) {
    try {
      const query = `
        SELECT 
          id, username, password_hash as "passwordHash", 
          email, roles, is_active as "isActive", 
          created_at as "createdAt", updated_at as "updatedAt"
        FROM users 
        WHERE username = $1
      `;
      
      const result = await pool.query(query, [username]);
      
      return result.rows[0] || null;
    } catch (error) {
      logger.error('Error finding user by username', {
        error: error.message,
        username
      });
      throw error;
    }
  }

  /**
   * Find user by ID
   * @param {string} userId - User ID
   * @returns {Object|null} User object or null
   */
  async findById(userId) {
    try {
      const query = `
        SELECT 
          id, username, email, roles, is_active as "isActive",
          created_at as "createdAt", updated_at as "updatedAt"
        FROM users 
        WHERE id = $1
      `;
      
      const result = await pool.query(query, [userId]);
      
      return result.rows[0] || null;
    } catch (error) {
      logger.error('Error finding user by ID', {
        error: error.message,
        userId
      });
      throw error;
    }
  }

  /**
   * Create new user
   * @param {Object} userData - User data
   * @returns {Object} Created user
   */
  async create(userData) {
    try {
      const query = `
        INSERT INTO users (username, password_hash, email, roles)
        VALUES ($1, $2, $3, $4)
        RETURNING id, username, email, roles, created_at as "createdAt"
      `;
      
      const values = [
        userData.username,
        userData.passwordHash,
        userData.email,
        userData.roles
      ];
      
      const result = await pool.query(query, values);
      
      logger.info('User created', { userId: result.rows[0].id });
      
      return result.rows[0];
    } catch (error) {
      logger.error('Error creating user', {
        error: error.message,
        username: userData.username
      });
      throw error;
    }
  }

  /**
   * Update user
   * @param {string} userId - User ID
   * @param {Object} updates - Fields to update
   * @returns {Object} Updated user
   */
  async update(userId, updates) {
    try {
      const query = `
        UPDATE users
        SET email = COALESCE($2, email),
            roles = COALESCE($3, roles),
            updated_at = NOW()
        WHERE id = $1
        RETURNING id, username, email, roles, updated_at as "updatedAt"
      `;
      
      const values = [userId, updates.email, updates.roles];
      const result = await pool.query(query, values);
      
      logger.info('User updated', { userId });
      
      return result.rows[0];
    } catch (error) {
      logger.error('Error updating user', {
        error: error.message,
        userId
      });
      throw error;
    }
  }
}

module.exports = new UserRepository();
```

### 3.4 Middleware

**Purpose**: Cross-cutting concerns (auth, logging, error handling)

**Example**: `src/middleware/auth.middleware.js`

```javascript
/**
 * Authentication Middleware
 * Implements: SEC-AUTH-001
 */

const authService = require('../services/auth.service');
const logger = require('../utils/logger');

/**
 * Verify JWT token and authenticate request
 */
async function authenticate(req, res, next) {
  try {
    // Extract token from Authorization header
    const authHeader = req.headers.authorization;
    
    if (!authHeader || !authHeader.startsWith('Bearer ')) {
      return res.status(401).json({
        error: 'Authentication required',
        message: 'No token provided'
      });
    }

    const token = authHeader.substring(7);

    // Verify token
    const decoded = await authService.verifyToken(token);

    // Attach user to request
    req.user = {
      id: decoded.userId,
      username: decoded.username,
      roles: decoded.roles
    };

    next();
  } catch (error) {
    logger.error('Authentication failed', {
      error: error.message,
      path: req.path
    });
    
    res.status(401).json({
      error: 'Authentication failed',
      message: error.message
    });
  }
}

/**
 * Authorize based on roles
 * @param {Array<string>} allowedRoles - Roles that can access the route
 */
function authorize(allowedRoles) {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({
        error: 'Authentication required'
      });
    }

    const hasRole = req.user.roles.some(role => allowedRoles.includes(role));

    if (!hasRole) {
      return res.status(403).json({
        error: 'Insufficient permissions',
        required: allowedRoles,
        current: req.user.roles
      });
    }

    next();
  };
}

module.exports = {
  authenticate,
  authorize
};
```

### 3.5 Configuration

**Purpose**: Centralized configuration management

**Example**: `src/config/app.js`

```javascript
/**
 * Application Configuration
 */

const dotenv = require('dotenv');

// Load environment variables
dotenv.config();

/**
 * Validate required environment variables
 */
function validateConfig() {
  const required = [
    'PORT',
    'NODE_ENV',
    'JWT_SECRET',
    'DATABASE_URL',
    'REDIS_URL'
  ];

  const missing = required.filter(key => !process.env[key]);

  if (missing.length > 0) {
    throw new Error(`Missing required environment variables: ${missing.join(', ')}`);
  }
}

validateConfig();

module.exports = {
  // Application
  port: process.env.PORT || 3000,
  env: process.env.NODE_ENV || 'development',
  
  // Authentication (implements SEC-AUTH-001)
  jwtSecret: process.env.JWT_SECRET,
  jwtExpiresIn: process.env.JWT_EXPIRES_IN || '1h',
  sessionTimeout: parseInt(process.env.SESSION_TIMEOUT) || 1800, // 30 minutes (implements NFR-012)
  
  // Database (RES-AZ-002)
  database: {
    url: process.env.DATABASE_URL,
    poolMin: parseInt(process.env.DB_POOL_MIN) || 2,
    poolMax: parseInt(process.env.DB_POOL_MAX) || 10,
  },
  
  // Redis (RES-AZ-003)
  redis: {
    url: process.env.REDIS_URL,
  },
  
  // Logging
  logging: {
    level: process.env.LOG_LEVEL || 'info',
    format: process.env.LOG_FORMAT || 'json'
  },
  
  // Security
  security: {
    bcryptRounds: parseInt(process.env.BCRYPT_ROUNDS) || 10,
    rateLimitWindowMs: parseInt(process.env.RATE_LIMIT_WINDOW) || 900000, // 15 minutes
    rateLimitMaxRequests: parseInt(process.env.RATE_LIMIT_MAX) || 100
  }
};
```

---

## 4. Testing

### 4.1 Unit Tests

**Example**: `tests/unit/services/auth.service.test.js`

```javascript
/**
 * Authentication Service Unit Tests
 * Tests: COMP-APP-001, FR-001
 */

const authService = require('../../../src/services/auth.service');
const userRepository = require('../../../src/repositories/user.repository');
const redisClient = require('../../../src/config/redis');
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');

// Mock dependencies
jest.mock('../../../src/repositories/user.repository');
jest.mock('../../../src/config/redis');
jest.mock('bcrypt');
jest.mock('jsonwebtoken');

describe('COMP-APP-001: Authentication Service', () => {
  describe('authenticate()', () => {
    it('should return token for valid credentials (implements FR-001)', async () => {
      // Arrange
      const mockUser = {
        id: '123',
        username: 'testuser',
        passwordHash: 'hashedpassword',
        roles: ['user'],
        isActive: true
      };

      userRepository.findByUsername.mockResolvedValue(mockUser);
      bcrypt.compare.mockResolvedValue(true);
      jwt.sign.mockReturnValue('mock-jwt-token');
      redisClient.setex.mockResolvedValue('OK');

      // Act
      const result = await authService.authenticate('testuser', 'password123');

      // Assert
      expect(result).toHaveProperty('token', 'mock-jwt-token');
      expect(result).toHaveProperty('user');
      expect(result.user.id).toBe('123');
      expect(userRepository.findByUsername).toHaveBeenCalledWith('testuser');
      expect(redisClient.setex).toHaveBeenCalled();
    });

    it('should throw error for invalid username', async () => {
      // Arrange
      userRepository.findByUsername.mockResolvedValue(null);

      // Act & Assert
      await expect(
        authService.authenticate('invaliduser', 'password123')
      ).rejects.toThrow('Invalid credentials');
    });

    it('should throw error for invalid password', async () => {
      // Arrange
      const mockUser = {
        id: '123',
        username: 'testuser',
        passwordHash: 'hashedpassword',
        isActive: true
      };

      userRepository.findByUsername.mockResolvedValue(mockUser);
      bcrypt.compare.mockResolvedValue(false);

      // Act & Assert
      await expect(
        authService.authenticate('testuser', 'wrongpassword')
      ).rejects.toThrow('Invalid credentials');
    });

    it('should throw error for inactive user', async () => {
      // Arrange
      const mockUser = {
        id: '123',
        username: 'testuser',
        passwordHash: 'hashedpassword',
        isActive: false
      };

      userRepository.findByUsername.mockResolvedValue(mockUser);
      bcrypt.compare.mockResolvedValue(true);

      // Act & Assert
      await expect(
        authService.authenticate('testuser', 'password123')
      ).rejects.toThrow('Account is inactive');
    });

    it('should store session in Redis (implements NFR-012)', async () => {
      // Arrange
      const mockUser = {
        id: '123',
        username: 'testuser',
        passwordHash: 'hashedpassword',
        roles: ['user'],
        isActive: true
      };

      userRepository.findByUsername.mockResolvedValue(mockUser);
      bcrypt.compare.mockResolvedValue(true);
      jwt.sign.mockReturnValue('mock-jwt-token');
      redisClient.setex.mockResolvedValue('OK');

      // Act
      await authService.authenticate('testuser', 'password123');

      // Assert
      expect(redisClient.setex).toHaveBeenCalledWith(
        'session:123',
        expect.any(Number),
        expect.any(String)
      );
    });
  });

  describe('logout()', () => {
    it('should remove session from Redis', async () => {
      // Arrange
      redisClient.del.mockResolvedValue(1);

      // Act
      await authService.logout('123');

      // Assert
      expect(redisClient.del).toHaveBeenCalledWith('session:123');
    });
  });

  describe('verifyToken()', () => {
    it('should verify valid token', async () => {
      // Arrange
      const mockDecoded = {
        userId: '123',
        username: 'testuser',
        roles: ['user']
      };

      jwt.verify.mockReturnValue(mockDecoded);
      redisClient.get.mockResolvedValue(JSON.stringify({ userId: '123' }));

      // Act
      const result = await authService.verifyToken('valid-token');

      // Assert
      expect(result).toEqual(mockDecoded);
      expect(jwt.verify).toHaveBeenCalled();
      expect(redisClient.get).toHaveBeenCalledWith('session:123');
    });

    it('should throw error for expired session', async () => {
      // Arrange
      const mockDecoded = { userId: '123' };
      jwt.verify.mockReturnValue(mockDecoded);
      redisClient.get.mockResolvedValue(null);

      // Act & Assert
      await expect(
        authService.verifyToken('valid-token')
      ).rejects.toThrow('Session expired');
    });
  });
});
```

### 4.2 Integration Tests

**Example**: `tests/integration/auth.test.js`

```javascript
/**
 * Authentication Integration Tests
 * Tests: Full authentication flow with database
 */

const request = require('supertest');
const app = require('../../src/index');
const { pool } = require('../../src/config/database');
const redisClient = require('../../src/config/redis');

describe('COMP-APP-001: Authentication API Integration Tests', () => {
  beforeAll(async () => {
    // Set up test database
    await pool.query('DELETE FROM users WHERE username LIKE \'test%\'');
  });

  afterAll(async () => {
    // Clean up
    await pool.end();
    await redisClient.quit();
  });

  describe('POST /auth/login', () => {
    beforeEach(async () => {
      // Create test user
      await pool.query(`
        INSERT INTO users (username, password_hash, email, roles, is_active)
        VALUES ($1, $2, $3, $4, $5)
      `, ['testuser', '$2b$10$hashedpassword', 'test@example.com', ['user'], true]);
    });

    afterEach(async () => {
      // Clean up test user
      await pool.query('DELETE FROM users WHERE username = $1', ['testuser']);
    });

    it('should authenticate user and return token (FR-001)', async () => {
      const response = await request(app)
        .post('/auth/login')
        .send({
          username: 'testuser',
          password: 'password123'
        });

      expect(response.status).toBe(200);
      expect(response.body).toHaveProperty('token');
      expect(response.body).toHaveProperty('user');
      expect(response.body.user.username).toBe('testuser');
    });

    it('should return 400 for missing credentials', async () => {
      const response = await request(app)
        .post('/auth/login')
        .send({ username: 'testuser' });

      expect(response.status).toBe(400);
      expect(response.body).toHaveProperty('error');
    });

    it('should return 401 for invalid credentials', async () => {
      const response = await request(app)
        .post('/auth/login')
        .send({
          username: 'testuser',
          password: 'wrongpassword'
        });

      expect(response.status).toBe(401);
    });
  });

  describe('POST /auth/logout', () => {
    it('should logout user and invalidate session', async () => {
      // First login
      const loginResponse = await request(app)
        .post('/auth/login')
        .send({
          username: 'testuser',
          password: 'password123'
        });

      const token = loginResponse.body.token;

      // Then logout
      const response = await request(app)
        .post('/auth/logout')
        .set('Authorization', `Bearer ${token}`);

      expect(response.status).toBe(200);

      // Verify session is deleted from Redis
      const session = await redisClient.get(`session:${loginResponse.body.user.id}`);
      expect(session).toBeNull();
    });
  });
});
```

---

## 5. Documentation

### 5.1 README.md

```markdown
# [COMP-APP-001] Authentication Service

## Overview

Authentication service that handles user authentication via Azure AD integration, session management, and JWT token generation.

**TBD References**:
- **Implements**: FR-001, NFR-010, NFR-012
- **Component ID**: COMP-APP-001
- **Resource ID**: RES-AZ-001
- **Task ID**: TASK-001

## Prerequisites

- Node.js 18 LTS
- PostgreSQL 14
- Redis 7
- Azure AD tenant (for production)

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd auth-service
```

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables:
```bash
cp .env.example .env
# Edit .env with your configuration
```

4. Run database migrations:
```bash
npm run migrate
```

## Configuration

### Environment Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `PORT` | Server port | Yes | 3000 |
| `NODE_ENV` | Environment (development/production) | Yes | development |
| `JWT_SECRET` | Secret for JWT signing | Yes | - |
| `JWT_EXPIRES_IN` | JWT expiration time | No | 1h |
| `SESSION_TIMEOUT` | Session timeout in seconds | No | 1800 |
| `DATABASE_URL` | PostgreSQL connection string | Yes | - |
| `REDIS_URL` | Redis connection string | Yes | - |

See `.env.example` for complete list.

## Running the Application

### Development
```bash
npm run dev
```

### Production
```bash
npm start
```

### Docker
```bash
docker build -t auth-service:latest .
docker run -p 3000:3000 --env-file .env auth-service:latest
```

## Testing

### Run all tests
```bash
npm test
```

### Run unit tests only
```bash
npm run test:unit
```

### Run integration tests only
```bash
npm run test:integration
```

### Check coverage
```bash
npm run test:coverage
```

## API Documentation

See [API.md](docs/API.md) for complete API documentation.

### Quick Reference

#### POST /auth/login
Authenticate user and receive JWT token.

```bash
curl -X POST http://localhost:3000/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "user", "password": "pass"}'
```

#### POST /auth/logout
Logout user and invalidate session.

```bash
curl -X POST http://localhost:3000/auth/logout \
  -H "Authorization: Bearer <token>"
```

## Health Check

```bash
curl http://localhost:3000/health
```

## Troubleshooting

### Database Connection Errors
- Verify PostgreSQL is running
- Check `DATABASE_URL` in `.env`
- Ensure database exists and migrations are run

### Redis Connection Errors
- Verify Redis is running
- Check `REDIS_URL` in `.env`

### Authentication Failures
- Check JWT_SECRET is set
- Verify user exists in database
- Check Redis for active sessions

## Development

### Code Style
- ESLint for linting
- Prettier for formatting

```bash
npm run lint
npm run format
```

### Database Migrations
```bash
npm run migrate:up      # Run migrations
npm run migrate:down    # Rollback migrations
npm run migrate:create  # Create new migration
```

## Deployment

See deployment documentation in Stage 3.

## License

[Your License]
```

### 5.2 API Documentation (OpenAPI)

**File**: `docs/API.md`

```yaml
openapi: 3.0.0
info:
  title: Authentication Service API
  description: |
    Authentication service API implementing Azure AD authentication.
    
    **TBD References**:
    - Component: COMP-APP-001
    - Implements: FR-001, NFR-010, NFR-012
    - Resource: RES-AZ-001
  version: 1.0.0
  contact:
    name: API Support
    email: support@example.com

servers:
  - url: http://localhost:3000
    description: Development server
  - url: https://api.example.com
    description: Production server

tags:
  - name: Authentication
    description: User authentication operations

paths:
  /auth/login:
    post:
      tags:
        - Authentication
      summary: Authenticate user
      description: |
        Authenticate user with username and password. Returns JWT token for subsequent requests.
        
        Implements: FR-001 (Azure AD Authentication), SEC-AUTH-001
      operationId: login
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoginRequest'
            examples:
              valid:
                summary: Valid credentials
                value:
                  username: johndoe
                  password: SecurePassword123!
      responses:
        '200':
          description: Authentication successful
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/LoginResponse'
              examples:
                success:
                  summary: Successful authentication
                  value:
                    token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
                    expiresIn: 3600
                    user:
                      id: "123"
                      username: johndoe
                      roles: ["user"]
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '500':
          $ref: '#/components/responses/InternalError'

  /auth/logout:
    post:
      tags:
        - Authentication
      summary: Logout user
      description: |
        Logout user and invalidate session.
        
        Implements: FR-001, NFR-012 (Session timeout)
      operationId: logout
      security:
        - bearerAuth: []
      responses:
        '200':
          description: Logout successful
          content:
            application/json:
              schema:
                type: object
                properties:
                  message:
                    type: string
                    example: Logged out successfully
        '401':
          $ref: '#/components/responses/Unauthorized'
        '500':
          $ref: '#/components/responses/InternalError'

  /auth/verify:
    get:
      tags:
        - Authentication
      summary: Verify token
      description: Verify if the provided JWT token is valid
      operationId: verifyToken
      security:
        - bearerAuth: []
      responses:
        '200':
          description: Token is valid
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/TokenVerifyResponse'
        '401':
          $ref: '#/components/responses/Unauthorized'

  /health:
    get:
      tags:
        - Health
      summary: Health check
      description: Check if service is healthy
      responses:
        '200':
          description: Service is healthy
          content:
            application/json:
              schema:
                type: object
                properties:
                  status:
                    type: string
                    example: healthy
                  timestamp:
                    type: string
                    format: date-time
                  services:
                    type: object
                    properties:
                      database:
                        type: string
                        example: connected
                      redis:
                        type: string
                        example: connected

components:
  schemas:
    LoginRequest:
      type: object
      required:
        - username
        - password
      properties:
        username:
          type: string
          minLength: 3
          maxLength: 50
          example: johndoe
        password:
          type: string
          minLength: 8
          maxLength: 100
          format: password
          example: SecurePassword123!

    LoginResponse:
      type: object
      properties:
        token:
          type: string
          description: JWT token for authentication
          example: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
        expiresIn:
          type: integer
          description: Token expiration time in seconds
          example: 3600
        user:
          $ref: '#/components/schemas/User'

    User:
      type: object
      properties:
        id:
          type: string
          example: "123"
        username:
          type: string
          example: johndoe
        roles:
          type: array
          items:
            type: string
          example: ["user", "admin"]

    TokenVerifyResponse:
      type: object
      properties:
        valid:
          type: boolean
          example: true
        user:
          $ref: '#/components/schemas/User'

    Error:
      type: object
      properties:
        error:
          type: string
          description: Error type
        message:
          type: string
          description: Error message
        details:
          type: object
          description: Additional error details

  responses:
    BadRequest:
      description: Bad request
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: Validation error
            message: Invalid input data
            details:
              field: username
              message: Username is required

    Unauthorized:
      description: Unauthorized
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: Authentication failed
            message: Invalid credentials

    InternalError:
      description: Internal server error
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            error: Internal error
            message: An unexpected error occurred

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: JWT token obtained from /auth/login endpoint
```

---

## 6. Containerization

### Dockerfile

```dockerfile
# Multi-stage build for optimized image size
# Component: COMP-APP-001
# Resource: RES-AZ-001

# Stage 1: Build
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy source code
COPY . .

# Stage 2: Production
FROM node:18-alpine

# Add non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

WORKDIR /app

# Copy from builder
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
COPY --chown=nodejs:nodejs . .

# Switch to non-root user
USER nodejs

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=40s --retries=3 \
  CMD node -e "require('http').get('http://localhost:3000/health', (r) => {process.exit(r.statusCode === 200 ? 0 : 1)})"

# Start application
CMD ["node", "src/index.js"]

# Labels
LABEL maintainer="team@example.com"
LABEL component="COMP-APP-001"
LABEL resource="RES-AZ-001"
LABEL version="1.0.0"
```

### docker-compose.yml

```yaml
version: '3.8'

services:
  auth-service:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
      - PORT=3000
      - DATABASE_URL=postgresql://postgres:postgres@postgres:5432/authdb
      - REDIS_URL=redis://redis:6379
      - JWT_SECRET=dev-secret-change-in-production
    depends_on:
      - postgres
      - redis
    volumes:
      - ./src:/app/src
    command: npm run dev

  postgres:
    image: postgres:14-alpine
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=authdb
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

---

## 7. Implementation Checklist

### Planning
- [ ] Review TBD specifications
- [ ] Understand requirements (FR-XXX, NFR-XXX)
- [ ] Review component dependencies
- [ ] Set up development environment

### Implementation
- [ ] Create project structure
- [ ] Implement configuration management
- [ ] Implement data models
- [ ] Implement repositories (data access)
- [ ] Implement services (business logic)
- [ ] Implement controllers (HTTP handlers)
- [ ] Implement middleware (auth, logging, errors)
- [ ] Implement routes
- [ ] Implement health check endpoint
- [ ] Implement error handling
- [ ] Implement logging

### Testing
- [ ] Write unit tests for services
- [ ] Write unit tests for repositories
- [ ] Write unit tests for controllers
- [ ] Write integration tests
- [ ] Achieve ≥80% code coverage
- [ ] All tests pass
- [ ] Performance tests (if applicable)

### Documentation
- [ ] Write README.md
- [ ] Write API documentation (OpenAPI)
- [ ] Write setup guide
- [ ] Document configuration options
- [ ] Add inline code comments
- [ ] Create architecture documentation

### Containerization
- [ ] Create Dockerfile
- [ ] Create docker-compose.yml
- [ ] Build image successfully
- [ ] Test container locally
- [ ] Optimize image size
- [ ] Add health check

### Quality
- [ ] Code follows style guide
- [ ] No hardcoded secrets
- [ ] All linting passes
- [ ] No security vulnerabilities
- [ ] Error handling is comprehensive
- [ ] Logging is consistent

### Integration
- [ ] Integrate with database
- [ ] Integrate with Redis
- [ ] Integrate with external APIs (if applicable)
- [ ] Test all integrations

### Review
- [ ] Self-review code
- [ ] Create pull request
- [ ] Address review comments
- [ ] Get approval
- [ ] Merge to main branch

---

## 8. Additional Notes

### Performance Considerations
- Use connection pooling for database
- Implement caching for frequently accessed data
- Use async/await for all I/O operations
- Add indexes to database queries

### Security Considerations
- Never log sensitive data (passwords, tokens)
- Use parameterized queries to prevent SQL injection
- Validate and sanitize all inputs
- Use HTTPS in production
- Rotate secrets regularly

### Monitoring & Observability
- Structured logging (JSON format)
- Log levels: error, warn, info, debug
- Include correlation IDs for request tracing
- Export metrics (response times, error rates)

---

**Template Version**: 1.0  
**Last Updated**: [DATE]

[Back to Stage 2 Overview](README.md) | [Stage 2 Prompt](PROMPT.md)
