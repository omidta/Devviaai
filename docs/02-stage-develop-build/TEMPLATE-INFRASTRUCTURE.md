# Infrastructure as Code Template

**Resource ID**: [RES-AZ-XXX]  
**Resource Name**: [Resource Name]  
**Version**: 1.0  
**Status**: Draft | In Development | Complete

## Metadata

| Field | Value |
|-------|-------|
| **Resource ID** | RES-AZ-XXX |
| **Azure Service** | [Service Type] |
| **Component** | COMP-XXX |
| **Implements Requirements** | FR-XXX, NFR-XXX |
| **Task ID** | TASK-XXX |
| **Environment** | Dev, Staging, Production |

---

## 1. Overview

### Purpose
[Brief description of what this infrastructure provides]

### Azure Resources
- [Resource 1]
- [Resource 2]
- [Resource 3]

### Dependencies
- [Dependency 1]
- [Dependency 2]

---

## 2. Module Structure

```
infrastructure/
├── modules/
│   └── [resource-type]/
│       ├── main.tf              # Resource definitions
│       ├── variables.tf         # Input variables
│       ├── outputs.tf           # Output values
│       ├── versions.tf          # Provider versions
│       ├── locals.tf            # Local values
│       └── README.md            # Module documentation
├── environments/
│   ├── dev/
│   │   ├── main.tf              # Dev environment config
│   │   ├── variables.tfvars     # Dev variables
│   │   ├── backend.tf           # Dev state backend
│   │   └── terraform.tfvars     # Auto-loaded variables
│   ├── staging/
│   │   ├── main.tf
│   │   ├── variables.tfvars
│   │   ├── backend.tf
│   │   └── terraform.tfvars
│   └── production/
│       ├── main.tf
│       ├── variables.tfvars
│       ├── backend.tf
│       └── terraform.tfvars
├── scripts/
│   ├── deploy.sh                # Deployment script
│   ├── destroy.sh               # Destroy script
│   └── validate.sh              # Validation script
├── tests/
│   └── terraform_test.go        # Terratest unit tests
├── .terraform.lock.hcl          # Dependency lock file
└── README.md                    # Main documentation
```

---

## 3. Terraform Implementation

### 3.1 Module: Main Resources

**File**: `modules/[resource-type]/main.tf`

```hcl
# RES-AZ-001: Web Application Service
# Component: COMP-APP-001
# Implements: FR-001, NFR-001, NFR-004
# Task: TASK-007

terraform {
  required_version = ">= 1.5.0"
  
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
}

# Resource Group
resource "azurerm_resource_group" "main" {
  name     = "${var.project_name}-${var.environment}-rg"
  location = var.location

  tags = merge(
    var.common_tags,
    {
      Resource    = "RES-AZ-001"
      Component   = "COMP-APP-001"
      Environment = var.environment
    }
  )
}

# App Service Plan
# Implements: NFR-007 (Scalability)
resource "azurerm_service_plan" "main" {
  name                = "${var.project_name}-${var.environment}-asp"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  
  os_type  = "Linux"
  sku_name = var.app_service_plan_sku

  tags = merge(
    var.common_tags,
    {
      Resource    = "RES-AZ-001"
      Component   = "COMP-APP-001"
      Requirement = "NFR-007"
    }
  )
}

# App Service
# Implements: FR-001, NFR-001 (Performance), NFR-004 (Availability)
resource "azurerm_linux_web_app" "main" {
  name                = "${var.project_name}-${var.environment}-app"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  service_plan_id     = azurerm_service_plan.main.id

  # HTTPS only (SEC-DATA-002: Encryption in transit)
  https_only = true

  site_config {
    # Always on for production (NFR-004: Availability)
    always_on = var.environment == "production" ? true : false

    # Health check endpoint (NFR-004: Availability)
    health_check_path = "/health"
    
    # Application stack
    application_stack {
      node_version = var.node_version
    }

    # Minimum TLS version (SEC-DATA-002)
    minimum_tls_version = "1.2"

    # CORS settings
    cors {
      allowed_origins = var.allowed_origins
    }

    # Auto heal enabled
    auto_heal_enabled = true
    auto_heal_setting {
      trigger {
        status_code {
          status_code_range = "500-599"
          count             = 10
          interval          = "00:01:00"
        }
      }
      action {
        action_type = "Recycle"
      }
    }
  }

  # Application settings (environment variables)
  app_settings = merge(
    {
      "NODE_ENV"                     = var.environment
      "WEBSITES_PORT"                = "3000"
      "WEBSITE_NODE_DEFAULT_VERSION" = var.node_version
      
      # Key Vault references (SEC-DATA-003: Secret Management)
      "DATABASE_URL"     = "@Microsoft.KeyVault(SecretUri=${azurerm_key_vault_secret.database_url.id})"
      "REDIS_URL"        = "@Microsoft.KeyVault(SecretUri=${azurerm_key_vault_secret.redis_url.id})"
      "JWT_SECRET"       = "@Microsoft.KeyVault(SecretUri=${azurerm_key_vault_secret.jwt_secret.id})"
      
      # Application Insights (Stage 4: Monitoring)
      "APPLICATIONINSIGHTS_CONNECTION_STRING" = azurerm_application_insights.main.connection_string
      
      # Logging
      "LOG_LEVEL"  = var.log_level
      "LOG_FORMAT" = "json"
    },
    var.additional_app_settings
  )

  # Connection strings
  connection_string {
    name  = "Database"
    type  = "PostgreSQL"
    value = "@Microsoft.KeyVault(SecretUri=${azurerm_key_vault_secret.database_url.id})"
  }

  # Managed identity (for Key Vault access)
  identity {
    type = "SystemAssigned"
  }

  # Logs
  logs {
    detailed_error_messages = true
    failed_request_tracing  = true

    application_logs {
      file_system_level = "Information"
    }

    http_logs {
      file_system {
        retention_in_days = var.log_retention_days
        retention_in_mb   = 35
      }
    }
  }

  tags = merge(
    var.common_tags,
    {
      Resource    = "RES-AZ-001"
      Component   = "COMP-APP-001"
      Requirement = "FR-001,NFR-001,NFR-004"
    }
  )
}

# Auto-scaling settings
# Implements: NFR-007 (Scalability)
resource "azurerm_monitor_autoscale_setting" "main" {
  name                = "${var.project_name}-${var.environment}-autoscale"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  target_resource_id  = azurerm_service_plan.main.id

  profile {
    name = "defaultProfile"

    capacity {
      default = var.autoscale_default_instances
      minimum = var.autoscale_min_instances
      maximum = var.autoscale_max_instances
    }

    # Scale out when CPU > 70%
    rule {
      metric_trigger {
        metric_name        = "CpuPercentage"
        metric_resource_id = azurerm_service_plan.main.id
        time_grain         = "PT1M"
        statistic          = "Average"
        time_window        = "PT5M"
        time_aggregation   = "Average"
        operator           = "GreaterThan"
        threshold          = 70
      }

      scale_action {
        direction = "Increase"
        type      = "ChangeCount"
        value     = "1"
        cooldown  = "PT5M"
      }
    }

    # Scale in when CPU < 30%
    rule {
      metric_trigger {
        metric_name        = "CpuPercentage"
        metric_resource_id = azurerm_service_plan.main.id
        time_grain         = "PT1M"
        statistic          = "Average"
        time_window        = "PT10M"
        time_aggregation   = "Average"
        operator           = "LessThan"
        threshold          = 30
      }

      scale_action {
        direction = "Decrease"
        type      = "ChangeCount"
        value     = "1"
        cooldown  = "PT10M"
      }
    }

    # Scale out when Memory > 75%
    rule {
      metric_trigger {
        metric_name        = "MemoryPercentage"
        metric_resource_id = azurerm_service_plan.main.id
        time_grain         = "PT1M"
        statistic          = "Average"
        time_window        = "PT5M"
        time_aggregation   = "Average"
        operator           = "GreaterThan"
        threshold          = 75
      }

      scale_action {
        direction = "Increase"
        type      = "ChangeCount"
        value     = "1"
        cooldown  = "PT5M"
      }
    }
  }

  notification {
    email {
      send_to_subscription_administrator    = false
      send_to_subscription_co_administrator = false
      custom_emails                         = var.alert_email_addresses
    }
  }

  tags = merge(
    var.common_tags,
    {
      Resource    = "RES-AZ-001"
      Requirement = "NFR-007"
    }
  )
}

# Application Insights
# Implements: Stage 4 monitoring requirements
resource "azurerm_application_insights" "main" {
  name                = "${var.project_name}-${var.environment}-ai"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  application_type    = "Node.JS"
  retention_in_days   = var.appinsights_retention_days

  tags = merge(
    var.common_tags,
    {
      Resource  = "RES-AZ-006"
      Component = "COMP-APP-001"
    }
  )
}

# Key Vault (for secrets)
# Implements: SEC-DATA-003 (Secret Management)
resource "azurerm_key_vault" "main" {
  name                       = "${var.project_name}-${var.environment}-kv"
  location                   = azurerm_resource_group.main.location
  resource_group_name        = azurerm_resource_group.main.name
  tenant_id                  = data.azurerm_client_config.current.tenant_id
  sku_name                   = "standard"
  soft_delete_retention_days = 7
  purge_protection_enabled   = var.environment == "production" ? true : false

  # Network ACLs (SEC-NET-001)
  network_acls {
    bypass         = "AzureServices"
    default_action = "Deny"
    
    ip_rules = var.allowed_ip_addresses
  }

  tags = merge(
    var.common_tags,
    {
      Resource    = "RES-AZ-005"
      Requirement = "SEC-DATA-003"
    }
  )
}

# Key Vault access policy for App Service
resource "azurerm_key_vault_access_policy" "app_service" {
  key_vault_id = azurerm_key_vault.main.id
  tenant_id    = data.azurerm_client_config.current.tenant_id
  object_id    = azurerm_linux_web_app.main.identity[0].principal_id

  secret_permissions = [
    "Get",
    "List"
  ]
}

# Key Vault secrets
resource "azurerm_key_vault_secret" "database_url" {
  name         = "database-url"
  value        = var.database_connection_string
  key_vault_id = azurerm_key_vault.main.id

  depends_on = [
    azurerm_key_vault_access_policy.app_service
  ]
}

resource "azurerm_key_vault_secret" "redis_url" {
  name         = "redis-url"
  value        = var.redis_connection_string
  key_vault_id = azurerm_key_vault.main.id

  depends_on = [
    azurerm_key_vault_access_policy.app_service
  ]
}

resource "azurerm_key_vault_secret" "jwt_secret" {
  name         = "jwt-secret"
  value        = var.jwt_secret
  key_vault_id = azurerm_key_vault.main.id

  depends_on = [
    azurerm_key_vault_access_policy.app_service
  ]
}

# Data source for current Azure client config
data "azurerm_client_config" "current" {}

# Private endpoint for App Service (optional, for production)
# Implements: SEC-NET-001 (Network Security)
resource "azurerm_private_endpoint" "app_service" {
  count               = var.enable_private_endpoint ? 1 : 0
  name                = "${var.project_name}-${var.environment}-pe"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
  subnet_id           = var.private_endpoint_subnet_id

  private_service_connection {
    name                           = "${var.project_name}-${var.environment}-psc"
    private_connection_resource_id = azurerm_linux_web_app.main.id
    is_manual_connection           = false
    subresource_names              = ["sites"]
  }

  tags = merge(
    var.common_tags,
    {
      Resource    = "RES-AZ-001"
      Requirement = "SEC-NET-001"
    }
  )
}
```

### 3.2 Module: Variables

**File**: `modules/[resource-type]/variables.tf`

```hcl
# Core variables
variable "project_name" {
  description = "Project name used for resource naming (implements naming convention)"
  type        = string
  
  validation {
    condition     = can(regex("^[a-z0-9-]+$", var.project_name))
    error_message = "Project name must contain only lowercase letters, numbers, and hyphens."
  }
}

variable "environment" {
  description = "Environment name (dev, staging, production)"
  type        = string
  
  validation {
    condition     = contains(["dev", "staging", "production"], var.environment)
    error_message = "Environment must be dev, staging, or production."
  }
}

variable "location" {
  description = "Azure region for resources"
  type        = string
  default     = "eastus"
}

# App Service Plan
variable "app_service_plan_sku" {
  description = "SKU for App Service Plan (RES-AZ-001)"
  type        = string
  default     = "P1v2"
  
  validation {
    condition     = can(regex("^(B[1-3]|S[1-3]|P[1-3]v[23]|I[1-3]v2)$", var.app_service_plan_sku))
    error_message = "Invalid App Service Plan SKU."
  }
}

# Application configuration
variable "node_version" {
  description = "Node.js version for the application"
  type        = string
  default     = "18-lts"
}

variable "log_level" {
  description = "Application log level"
  type        = string
  default     = "info"
  
  validation {
    condition     = contains(["error", "warn", "info", "debug"], var.log_level)
    error_message = "Log level must be error, warn, info, or debug."
  }
}

variable "log_retention_days" {
  description = "Number of days to retain application logs"
  type        = number
  default     = 30
}

variable "additional_app_settings" {
  description = "Additional application settings (environment variables)"
  type        = map(string)
  default     = {}
}

# Auto-scaling (implements NFR-007)
variable "autoscale_min_instances" {
  description = "Minimum number of instances for auto-scaling"
  type        = number
  default     = 2
  
  validation {
    condition     = var.autoscale_min_instances >= 1 && var.autoscale_min_instances <= 30
    error_message = "Minimum instances must be between 1 and 30."
  }
}

variable "autoscale_max_instances" {
  description = "Maximum number of instances for auto-scaling"
  type        = number
  default     = 5
  
  validation {
    condition     = var.autoscale_max_instances >= 1 && var.autoscale_max_instances <= 30
    error_message = "Maximum instances must be between 1 and 30."
  }
}

variable "autoscale_default_instances" {
  description = "Default number of instances"
  type        = number
  default     = 2
}

# Security (implements SEC-XXX)
variable "allowed_origins" {
  description = "CORS allowed origins"
  type        = list(string)
  default     = []
}

variable "allowed_ip_addresses" {
  description = "IP addresses allowed to access Key Vault"
  type        = list(string)
  default     = []
}

variable "enable_private_endpoint" {
  description = "Enable private endpoint for App Service (SEC-NET-001)"
  type        = bool
  default     = false
}

variable "private_endpoint_subnet_id" {
  description = "Subnet ID for private endpoint"
  type        = string
  default     = null
}

# Secrets
variable "database_connection_string" {
  description = "PostgreSQL database connection string (stored in Key Vault)"
  type        = string
  sensitive   = true
}

variable "redis_connection_string" {
  description = "Redis connection string (stored in Key Vault)"
  type        = string
  sensitive   = true
}

variable "jwt_secret" {
  description = "JWT signing secret (stored in Key Vault)"
  type        = string
  sensitive   = true
}

# Monitoring
variable "appinsights_retention_days" {
  description = "Application Insights data retention in days"
  type        = number
  default     = 90
}

variable "alert_email_addresses" {
  description = "Email addresses for monitoring alerts"
  type        = list(string)
  default     = []
}

# Tags
variable "common_tags" {
  description = "Common tags to apply to all resources"
  type        = map(string)
  default = {
    ManagedBy = "Terraform"
    Project   = "Devviaai"
  }
}
```

### 3.3 Module: Outputs

**File**: `modules/[resource-type]/outputs.tf`

```hcl
# Resource Group
output "resource_group_name" {
  description = "Name of the resource group"
  value       = azurerm_resource_group.main.name
}

output "resource_group_id" {
  description = "ID of the resource group"
  value       = azurerm_resource_group.main.id
}

# App Service
output "app_service_name" {
  description = "Name of the App Service (RES-AZ-001)"
  value       = azurerm_linux_web_app.main.name
}

output "app_service_id" {
  description = "ID of the App Service"
  value       = azurerm_linux_web_app.main.id
}

output "app_service_default_hostname" {
  description = "Default hostname of the App Service"
  value       = azurerm_linux_web_app.main.default_hostname
}

output "app_service_outbound_ip_addresses" {
  description = "Outbound IP addresses of the App Service"
  value       = azurerm_linux_web_app.main.outbound_ip_addresses
}

output "app_service_principal_id" {
  description = "Principal ID of the App Service managed identity"
  value       = azurerm_linux_web_app.main.identity[0].principal_id
}

# Application Insights
output "application_insights_instrumentation_key" {
  description = "Application Insights instrumentation key"
  value       = azurerm_application_insights.main.instrumentation_key
  sensitive   = true
}

output "application_insights_connection_string" {
  description = "Application Insights connection string"
  value       = azurerm_application_insights.main.connection_string
  sensitive   = true
}

output "application_insights_app_id" {
  description = "Application Insights application ID"
  value       = azurerm_application_insights.main.app_id
}

# Key Vault
output "key_vault_name" {
  description = "Name of the Key Vault (RES-AZ-005)"
  value       = azurerm_key_vault.main.name
}

output "key_vault_id" {
  description = "ID of the Key Vault"
  value       = azurerm_key_vault.main.id
}

output "key_vault_uri" {
  description = "URI of the Key Vault"
  value       = azurerm_key_vault.main.vault_uri
}

# Monitoring
output "autoscale_setting_id" {
  description = "ID of the autoscale setting"
  value       = azurerm_monitor_autoscale_setting.main.id
}

# Private Endpoint
output "private_endpoint_id" {
  description = "ID of the private endpoint (if enabled)"
  value       = var.enable_private_endpoint ? azurerm_private_endpoint.app_service[0].id : null
}

output "private_endpoint_ip_address" {
  description = "Private IP address of the private endpoint"
  value       = var.enable_private_endpoint ? azurerm_private_endpoint.app_service[0].private_service_connection[0].private_ip_address : null
}
```

### 3.4 Module: Provider Versions

**File**: `modules/[resource-type]/versions.tf`

```hcl
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
    random = {
      source  = "hashicorp/random"
      version = "~> 3.0"
    }
  }
}
```

### 3.5 Module: Local Values

**File**: `modules/[resource-type]/locals.tf`

```hcl
locals {
  # Common naming prefix
  name_prefix = "${var.project_name}-${var.environment}"

  # Common tags
  common_tags = merge(
    var.common_tags,
    {
      Environment = var.environment
      ManagedBy   = "Terraform"
      CreatedDate = timestamp()
    }
  )

  # Computed values
  is_production = var.environment == "production"
  is_development = var.environment == "dev"

  # Feature flags based on environment
  enable_advanced_security = local.is_production
  enable_backup           = local.is_production
  enable_monitoring       = true
}
```

---

## 4. Environment Configurations

### 4.1 Development Environment

**File**: `environments/dev/main.tf`

```hcl
terraform {
  required_version = ">= 1.5.0"

  backend "azurerm" {
    resource_group_name  = "terraform-state-rg"
    storage_account_name = "tfstatedevviaai"
    container_name       = "tfstate"
    key                  = "dev/terraform.tfstate"
  }

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
}

provider "azurerm" {
  features {
    key_vault {
      purge_soft_delete_on_destroy = true
    }
  }
}

# Use the module
module "app_service" {
  source = "../../modules/app-service"

  # Core
  project_name = var.project_name
  environment  = "dev"
  location     = var.location

  # App Service Plan
  app_service_plan_sku = "B1"  # Basic tier for dev

  # Auto-scaling
  autoscale_min_instances     = 1
  autoscale_max_instances     = 2
  autoscale_default_instances = 1

  # Configuration
  node_version  = "18-lts"
  log_level     = "debug"
  allowed_origins = ["*"]  # Allow all origins in dev

  # Secrets
  database_connection_string = var.database_connection_string
  redis_connection_string    = var.redis_connection_string
  jwt_secret                 = var.jwt_secret

  # Monitoring
  alert_email_addresses = var.alert_email_addresses

  # Security
  enable_private_endpoint = false  # No private endpoint in dev

  # Tags
  common_tags = {
    Environment = "Development"
    Project     = var.project_name
    ManagedBy   = "Terraform"
    CostCenter  = "Engineering"
  }
}

# Outputs
output "app_service_url" {
  description = "App Service URL"
  value       = "https://${module.app_service.app_service_default_hostname}"
}

output "application_insights_app_id" {
  description = "Application Insights App ID"
  value       = module.app_service.application_insights_app_id
}
```

**File**: `environments/dev/variables.tf`

```hcl
variable "project_name" {
  description = "Project name"
  type        = string
  default     = "devviaai"
}

variable "location" {
  description = "Azure region"
  type        = string
  default     = "eastus"
}

variable "database_connection_string" {
  description = "Database connection string"
  type        = string
  sensitive   = true
}

variable "redis_connection_string" {
  description = "Redis connection string"
  type        = string
  sensitive   = true
}

variable "jwt_secret" {
  description = "JWT secret"
  type        = string
  sensitive   = true
}

variable "alert_email_addresses" {
  description = "Alert email addresses"
  type        = list(string)
  default     = []
}
```

**File**: `environments/dev/terraform.tfvars`

```hcl
# Development environment configuration
project_name = "devviaai"
location     = "eastus"

# Alert emails
alert_email_addresses = [
  "dev-team@example.com"
]

# Note: Sensitive variables should be set via environment variables or secret management
# Example:
# export TF_VAR_database_connection_string="..."
# export TF_VAR_redis_connection_string="..."
# export TF_VAR_jwt_secret="..."
```

### 4.2 Production Environment

**File**: `environments/production/main.tf`

```hcl
terraform {
  required_version = ">= 1.5.0"

  backend "azurerm" {
    resource_group_name  = "terraform-state-rg"
    storage_account_name = "tfstatedevviaai"
    container_name       = "tfstate"
    key                  = "production/terraform.tfstate"
  }

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
}

provider "azurerm" {
  features {
    key_vault {
      purge_soft_delete_on_destroy = false  # Protect production secrets
    }
  }
}

# Use the module
module "app_service" {
  source = "../../modules/app-service"

  # Core
  project_name = var.project_name
  environment  = "production"
  location     = var.location

  # App Service Plan (P1v2 per TBD RES-AZ-001)
  app_service_plan_sku = "P1v2"

  # Auto-scaling (implements NFR-007)
  autoscale_min_instances     = 2  # HA requirement
  autoscale_max_instances     = 5
  autoscale_default_instances = 2

  # Configuration
  node_version  = "18-lts"
  log_level     = "info"
  log_retention_days = 90
  
  # CORS - specific origins only in production
  allowed_origins = var.allowed_origins

  # Secrets
  database_connection_string = var.database_connection_string
  redis_connection_string    = var.redis_connection_string
  jwt_secret                 = var.jwt_secret

  # Monitoring
  appinsights_retention_days = 90
  alert_email_addresses      = var.alert_email_addresses

  # Security (implements SEC-NET-001)
  enable_private_endpoint     = var.enable_private_endpoint
  private_endpoint_subnet_id  = var.private_endpoint_subnet_id
  allowed_ip_addresses        = var.allowed_ip_addresses

  # Tags
  common_tags = {
    Environment = "Production"
    Project     = var.project_name
    ManagedBy   = "Terraform"
    CostCenter  = "Operations"
    Compliance  = "Required"
  }
}

# Outputs
output "app_service_url" {
  description = "App Service URL"
  value       = "https://${module.app_service.app_service_default_hostname}"
}

output "application_insights_app_id" {
  description = "Application Insights App ID"
  value       = module.app_service.application_insights_app_id
}
```

**File**: `environments/production/terraform.tfvars`

```hcl
# Production environment configuration
project_name = "devviaai"
location     = "eastus"

# CORS
allowed_origins = [
  "https://portal.example.com",
  "https://app.example.com"
]

# Alert emails
alert_email_addresses = [
  "ops-team@example.com",
  "oncall@example.com"
]

# Security
allowed_ip_addresses = [
  "1.2.3.4",  # Office IP
  "5.6.7.8"   # VPN IP
]

enable_private_endpoint = true

# Note: Sensitive variables should be set via Azure Key Vault or CI/CD secrets
```

---

## 5. Deployment Scripts

### 5.1 Deployment Script

**File**: `scripts/deploy.sh`

```bash
#!/bin/bash
# Deployment script for Terraform infrastructure
# Usage: ./scripts/deploy.sh [environment]

set -e

ENVIRONMENT=${1:-dev}
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
ROOT_DIR="$(cd "$SCRIPT_DIR/.." && pwd)"
ENV_DIR="$ROOT_DIR/environments/$ENVIRONMENT"

echo "======================================"
echo "Deploying to: $ENVIRONMENT"
echo "======================================"

# Validate environment
if [ ! -d "$ENV_DIR" ]; then
  echo "Error: Environment '$ENVIRONMENT' not found"
  exit 1
fi

cd "$ENV_DIR"

# Initialize Terraform
echo "Initializing Terraform..."
terraform init -upgrade

# Validate configuration
echo "Validating Terraform configuration..."
terraform validate

# Plan deployment
echo "Planning deployment..."
terraform plan -out=tfplan

# Confirm deployment
if [ "$ENVIRONMENT" == "production" ]; then
  read -p "Deploy to PRODUCTION? (yes/no): " confirm
  if [ "$confirm" != "yes" ]; then
    echo "Deployment cancelled"
    exit 0
  fi
fi

# Apply deployment
echo "Applying deployment..."
terraform apply tfplan

# Clean up plan file
rm tfplan

echo "======================================"
echo "Deployment completed successfully!"
echo "======================================"

# Show outputs
terraform output
```

### 5.2 Validation Script

**File**: `scripts/validate.sh`

```bash
#!/bin/bash
# Validation script for Terraform configuration
# Usage: ./scripts/validate.sh [environment]

set -e

ENVIRONMENT=${1:-dev}
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
ROOT_DIR="$(cd "$SCRIPT_DIR/.." && pwd)"
ENV_DIR="$ROOT_DIR/environments/$ENVIRONMENT"

echo "======================================"
echo "Validating: $ENVIRONMENT"
echo "======================================"

cd "$ENV_DIR"

# Format check
echo "Checking Terraform formatting..."
terraform fmt -check -recursive

# Initialize
echo "Initializing Terraform..."
terraform init -backend=false

# Validate
echo "Validating configuration..."
terraform validate

# Lint (if tflint is installed)
if command -v tflint &> /dev/null; then
  echo "Running tflint..."
  tflint
fi

echo "======================================"
echo "Validation completed successfully!"
echo "======================================"
```

### 5.3 Destroy Script

**File**: `scripts/destroy.sh`

```bash
#!/bin/bash
# Destroy script for Terraform infrastructure
# Usage: ./scripts/destroy.sh [environment]

set -e

ENVIRONMENT=${1:-dev}
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
ROOT_DIR="$(cd "$SCRIPT_DIR/.." && pwd)"
ENV_DIR="$ROOT_DIR/environments/$ENVIRONMENT"

echo "======================================"
echo "WARNING: Destroying: $ENVIRONMENT"
echo "======================================"

# Validate environment
if [ ! -d "$ENV_DIR" ]; then
  echo "Error: Environment '$ENVIRONMENT' not found"
  exit 1
fi

# Production protection
if [ "$ENVIRONMENT" == "production" ]; then
  echo "ERROR: Cannot destroy production environment with this script"
  echo "Manual destruction required for safety"
  exit 1
fi

cd "$ENV_DIR"

# Confirm destruction
read -p "Are you sure you want to destroy $ENVIRONMENT? (type 'destroy'): " confirm
if [ "$confirm" != "destroy" ]; then
  echo "Destruction cancelled"
  exit 0
fi

# Destroy
echo "Destroying infrastructure..."
terraform destroy -auto-approve

echo "======================================"
echo "Infrastructure destroyed"
echo "======================================"
```

---

## 6. Testing

### 6.1 Terratest Example

**File**: `tests/terraform_test.go`

```go
package test

import (
	"testing"

	"github.com/gruntwork-io/terratest/modules/terraform"
	"github.com/stretchr/testify/assert"
)

func TestAppServiceModule(t *testing.T) {
	t.Parallel()

	terraformOptions := terraform.WithDefaultRetryableErrors(t, &terraform.Options{
		TerraformDir: "../environments/dev",
		Vars: map[string]interface{}{
			"project_name": "test-project",
			"location":     "eastus",
		},
	})

	defer terraform.Destroy(t, terraformOptions)

	terraform.InitAndApply(t, terraformOptions)

	// Validate outputs
	appServiceName := terraform.Output(t, terraformOptions, "app_service_name")
	assert.NotEmpty(t, appServiceName)

	resourceGroupName := terraform.Output(t, terraformOptions, "resource_group_name")
	assert.Contains(t, resourceGroupName, "test-project")
}
```

---

## 7. Documentation

### 7.1 Module README

**File**: `modules/[resource-type]/README.md`

```markdown
# App Service Module

## Overview

Terraform module for provisioning Azure App Service with best practices for security, monitoring, and scalability.

**TBD References**:
- **Resource ID**: RES-AZ-001
- **Component**: COMP-APP-001
- **Implements**: FR-001, NFR-001, NFR-004, NFR-007
- **Security**: SEC-AUTH-001, SEC-NET-001, SEC-DATA-002, SEC-DATA-003

## Features

- ✅ Linux App Service with Node.js support
- ✅ Auto-scaling based on CPU and memory
- ✅ Application Insights integration
- ✅ Key Vault integration for secrets
- ✅ Private endpoint support
- ✅ Auto-healing configuration
- ✅ Managed identity
- ✅ HTTPS enforcement
- ✅ Health check endpoint

## Usage

```hcl
module "app_service" {
  source = "./modules/app-service"

  project_name = "my-project"
  environment  = "production"
  location     = "eastus"

  app_service_plan_sku = "P1v2"

  autoscale_min_instances = 2
  autoscale_max_instances = 5

  database_connection_string = var.database_url
  redis_connection_string    = var.redis_url
  jwt_secret                 = var.jwt_secret

  common_tags = {
    Project = "MyProject"
  }
}
```

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.5.0 |
| azurerm | ~> 3.0 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| project_name | Project name for resource naming | `string` | n/a | yes |
| environment | Environment (dev/staging/production) | `string` | n/a | yes |
| location | Azure region | `string` | `eastus` | no |
| app_service_plan_sku | App Service Plan SKU | `string` | `P1v2` | no |
| autoscale_min_instances | Minimum instances | `number` | `2` | no |
| autoscale_max_instances | Maximum instances | `number` | `5` | no |
| database_connection_string | Database connection string | `string` | n/a | yes |
| redis_connection_string | Redis connection string | `string` | n/a | yes |
| jwt_secret | JWT signing secret | `string` | n/a | yes |

See [variables.tf](variables.tf) for complete list.

## Outputs

| Name | Description |
|------|-------------|
| app_service_name | Name of the App Service |
| app_service_default_hostname | Default hostname |
| application_insights_instrumentation_key | App Insights key |
| key_vault_name | Key Vault name |

See [outputs.tf](outputs.tf) for complete list.

## Examples

### Development Environment

```hcl
module "app_service" {
  source = "./modules/app-service"

  project_name             = "my-project"
  environment              = "dev"
  app_service_plan_sku     = "B1"
  autoscale_min_instances  = 1
  autoscale_max_instances  = 2
  enable_private_endpoint  = false
  
  # ... other variables
}
```

### Production Environment

```hcl
module "app_service" {
  source = "./modules/app-service"

  project_name             = "my-project"
  environment              = "production"
  app_service_plan_sku     = "P1v2"
  autoscale_min_instances  = 2
  autoscale_max_instances  = 10
  enable_private_endpoint  = true
  private_endpoint_subnet_id = azurerm_subnet.private.id
  
  # ... other variables
}
```

## Resources Created

- Azure Resource Group
- App Service Plan
- Linux Web App
- Autoscale Settings
- Application Insights
- Key Vault
- Key Vault Secrets
- Key Vault Access Policies
- Private Endpoint (optional)

## Security

- HTTPS enforced (TLS 1.2 minimum)
- Secrets stored in Key Vault
- Managed identity for Key Vault access
- Private endpoint support
- Network ACLs on Key Vault
- Auto-healing enabled

## Monitoring

- Application Insights integrated
- Custom auto-scaling rules
- Email alerts configured
- Application logs enabled
- HTTP logs enabled

## Cost Estimation

### Development
- App Service Plan (B1): ~$13/month
- Application Insights: ~$2/month
- Key Vault: ~$0.03/month
- **Total**: ~$15/month

### Production
- App Service Plan (P1v2 x2): ~$292/month
- Application Insights: ~$10/month
- Key Vault: ~$0.03/month
- **Total**: ~$302/month

## License

[Your License]
```

---

## 8. Implementation Checklist

### Planning
- [ ] Review TBD resource specifications
- [ ] Identify all Azure resources needed
- [ ] Understand dependencies
- [ ] Review security requirements

### Module Development
- [ ] Create module structure
- [ ] Implement main.tf with all resources
- [ ] Define variables.tf with validation
- [ ] Define outputs.tf
- [ ] Create versions.tf
- [ ] Create locals.tf
- [ ] Add comprehensive comments with TBD references

### Environment Configuration
- [ ] Create dev environment configuration
- [ ] Create staging environment configuration
- [ ] Create production environment configuration
- [ ] Configure remote state backends
- [ ] Set up state locking

### Scripts
- [ ] Create deployment script
- [ ] Create validation script
- [ ] Create destroy script (with production protection)
- [ ] Test all scripts

### Testing
- [ ] Write Terratest unit tests
- [ ] Test in development environment
- [ ] Validate with `terraform plan`
- [ ] Check with `terraform validate`
- [ ] Run `terraform fmt`
- [ ] Test auto-scaling
- [ ] Test security configurations

### Documentation
- [ ] Write module README
- [ ] Document all variables
- [ ] Document all outputs
- [ ] Provide usage examples
- [ ] Document cost estimates
- [ ] Create architecture diagrams

### Security
- [ ] No secrets in code
- [ ] Use Key Vault for sensitive data
- [ ] Configure managed identities
- [ ] Set up network security
- [ ] Enable encryption
- [ ] Configure access policies

### Quality
- [ ] Code follows Terraform best practices
- [ ] Variables have validation rules
- [ ] Resources have proper tags
- [ ] Outputs are documented
- [ ] State management is configured
- [ ] Linting passes (tflint)

### Review
- [ ] Self-review code
- [ ] Create pull request
- [ ] Address review comments
- [ ] Get approval
- [ ] Merge to main branch

---

## 9. Additional Resources

### Terraform Documentation
- [Azure Provider Documentation](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs)
- [Terraform Best Practices](https://www.terraform.io/docs/cloud/guides/recommended-practices/index.html)
- [Module Development](https://www.terraform.io/docs/language/modules/develop/index.html)

### Azure Documentation
- [App Service Documentation](https://docs.microsoft.com/en-us/azure/app-service/)
- [Key Vault Documentation](https://docs.microsoft.com/en-us/azure/key-vault/)
- [Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)

### Tools
- [Terraform](https://www.terraform.io/)
- [tflint](https://github.com/terraform-linters/tflint)
- [Terratest](https://terratest.gruntwork.io/)
- [terraform-docs](https://terraform-docs.io/)

---

**Template Version**: 1.0  
**Last Updated**: [DATE]

[Back to Stage 2 Overview](README.md) | [Stage 2 Prompt](PROMPT.md) | [Component Template](TEMPLATE-COMPONENT.md)
