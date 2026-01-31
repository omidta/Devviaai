# Monitoring Configuration Template

**Type**: Monitoring & Observability Setup  
**Platform**: Azure Monitor + Application Insights  
**Version**: 1.0  
**Status**: Production Ready

## Metadata

| Field | Value |
|-------|-------|
| **Purpose** | Complete monitoring and observability setup |
| **Components** | Application Insights, Log Analytics, Dashboards, Alerts |
| **NFRs Validated** | [NFR-XXX List] |
| **SLO Targets** | [SLO List] |

---

## 1. Application Insights Setup

### Configuration

```json
{
  "ApplicationInsights": {
    "ConnectionString": "${APPLICATIONINSIGHTS_CONNECTION_STRING}",
    "EnableAdaptiveSampling": true,
    "EnableDependencyTracking": true,
    "EnablePerformanceCounterCollection": true,
    "EnableQuickPulseMetricStream": true,
    "CloudRoleName": "auth-service",
    "SamplingSettings": {
      "IsEnabled": true,
      "MaxTelemetryItemsPerSecond": 5,
      "EvaluationInterval": "00:00:15",
      "InitialSamplingPercentage": 100
    }
  }
}
```

### Node.js Integration

```javascript
const appInsights = require('applicationinsights');

appInsights.setup(process.env.APPLICATIONINSIGHTS_CONNECTION_STRING)
  .setAutoDependencyCorrelation(true)
  .setAutoCollectRequests(true)
  .setAutoCollectPerformance(true, true)
  .setAutoCollectExceptions(true)
  .setAutoCollectDependencies(true)
  .setAutoCollectConsole(true, true)
  .setUseDiskRetryCaching(true)
  .setSendLiveMetrics(true)
  .start();

const client = appInsights.defaultClient;

// Track custom events
client.trackEvent({
  name: "UserRegistration",
  properties: {
    userId: user.id,
    method: "email"
  }
});

// Track custom metrics
client.trackMetric({
  name: "QueueLength",
  value: queue.length
});
```

---

## 2. Custom Metrics

### Business Metrics

```javascript
// Track business transactions
function trackTransaction(type, amount, status) {
  client.trackMetric({
    name: "BusinessTransaction",
    value: amount,
    properties: {
      type: type,  // 'purchase', 'refund', etc.
      status: status,
      timestamp: new Date().toISOString()
    }
  });
}

// Track active users
function trackActiveUsers(count) {
  client.trackMetric({
    name: "ActiveUsers",
    value: count
  });
}

// Track feature usage
function trackFeatureUsage(feature) {
  client.trackEvent({
    name: "FeatureUsed",
    properties: { feature: feature }
  });
}
```

### Application Metrics

```javascript
// Track cache performance
function trackCacheMetrics(hits, misses) {
  const hitRate = hits / (hits + misses) * 100;
  client.trackMetric({ name: "CacheHitRate", value: hitRate });
}

// Track queue metrics
function trackQueueMetrics(queueName, length, avgWait) {
  client.trackMetric({
    name: "QueueLength",
    value: length,
    properties: { queue: queueName }
  });
  client.trackMetric({
    name: "QueueWaitTime",
    value: avgWait,
    properties: { queue: queueName }
  });
}

// Track database connection pool
function trackConnectionPool(active, idle, total) {
  client.trackMetric({ name: "DBConnectionsActive", value: active });
  client.trackMetric({ name: "DBConnectionsIdle", value: idle });
  client.trackMetric({ name: "DBConnectionsTotal", value: total });
}
```

---

## 3. Monitoring Dashboards

### Dashboard 1: System Health

```kql
// Overall Success Rate (Last 1 hour)
requests
| where timestamp > ago(1h)
| summarize 
    Total = count(),
    Success = countif(success == true),
    Failed = countif(success == false)
| extend SuccessRate = round(Success * 100.0 / Total, 2)
| project SuccessRate, Total, Success, Failed

// Request Count by Result Code
requests
| where timestamp > ago(1h)
| summarize Count = count() by resultCode
| order by Count desc

// Availability Over Time
requests
| where timestamp > ago(24h)
| summarize 
    SuccessRate = round(countif(success==true) * 100.0 / count(), 2) 
    by bin(timestamp, 5m)
| render timechart

// Current Health Status
requests
| where timestamp > ago(5m)
| summarize 
    RequestCount = count(),
    AvgDuration = avg(duration),
    FailureCount = countif(success == false)
| extend HealthStatus = iff(FailureCount == 0 and AvgDuration < 1000, "Healthy", "Degraded")
| project HealthStatus, RequestCount, AvgDuration, FailureCount
```

### Dashboard 2: Performance Metrics

```kql
// Response Time Percentiles
requests
| where timestamp > ago(1h)
| summarize 
    P50 = percentile(duration, 50),
    P95 = percentile(duration, 95),
    P99 = percentile(duration, 99),
    Max = max(duration)
| project P50, P95, P99, Max

// Slow Requests (P95 > 1000ms)
requests
| where timestamp > ago(1h) and duration > 1000
| project timestamp, name, duration, resultCode, operation_Id
| order by duration desc
| take 100

// Performance by Endpoint
requests
| where timestamp > ago(1h)
| summarize 
    Count = count(),
    AvgDuration = avg(duration),
    P95Duration = percentile(duration, 95)
    by name
| order by P95Duration desc

// Performance Trend
requests
| where timestamp > ago(24h)
| summarize 
    AvgDuration = avg(duration),
    P95Duration = percentile(duration, 95)
    by bin(timestamp, 1h)
| render timechart
```

### Dashboard 3: Error Analysis

```kql
// Error Rate Over Time
requests
| where timestamp > ago(24h)
| summarize 
    ErrorRate = round(countif(success==false) * 100.0 / count(), 2)
    by bin(timestamp, 5m)
| render timechart

// Top Exceptions
exceptions
| where timestamp > ago(24h)
| summarize Count = count() by type, outerMessage
| order by Count desc
| take 20

// Failed Requests by Endpoint
requests
| where timestamp > ago(1h) and success == false
| summarize Count = count() by name, resultCode
| order by Count desc

// Exception Details
exceptions
| where timestamp > ago(1h)
| project timestamp, type, outerMessage, innermostMessage, operation_Name
| order by timestamp desc
| take 100
```

### Dashboard 4: Infrastructure Metrics

```kql
// CPU Usage
performanceCounters
| where timestamp > ago(1h) and name == "% Processor Time"
| summarize AvgCPU = avg(value) by bin(timestamp, 5m)
| render timechart

// Memory Usage
performanceCounters
| where timestamp > ago(1h) and name == "Available Bytes"
| extend AvailableGB = value / (1024*1024*1024)
| summarize AvgMemory = avg(AvailableGB) by bin(timestamp, 5m)
| render timechart

// Dependency Performance
dependencies
| where timestamp > ago(1h)
| summarize 
    Count = count(),
    AvgDuration = avg(duration),
    FailureCount = countif(success == false)
    by type, name
| order by FailureCount desc, AvgDuration desc

// Database Query Performance
dependencies
| where timestamp > ago(1h) and type == "SQL"
| summarize 
    Count = count(),
    AvgDuration = avg(duration),
    P95Duration = percentile(duration, 95)
    by data
| order by P95Duration desc
| take 20
```

### Dashboard 5: Business Metrics

```kql
// Active Users Over Time
customMetrics
| where timestamp > ago(24h) and name == "ActiveUsers"
| summarize AvgUsers = avg(value) by bin(timestamp, 1h)
| render timechart

// Transaction Volume
customMetrics
| where timestamp > ago(24h) and name == "BusinessTransaction"
| summarize TotalTransactions = sum(value) by bin(timestamp, 1h)
| render timechart

// Transaction Success Rate
customEvents
| where timestamp > ago(24h) and name == "BusinessTransaction"
| summarize 
    Total = count(),
    Success = countif(customDimensions.status == "success")
| extend SuccessRate = Success * 100.0 / Total
| project SuccessRate, Total, Success

// Feature Usage
customEvents
| where timestamp > ago(7d) and name == "FeatureUsed"
| summarize Count = count() by tostring(customDimensions.feature)
| order by Count desc
| take 10
```

---

## 4. Alert Rules

### Critical Alerts

#### Service Completely Down

```json
{
  "name": "service-completely-down",
  "description": "No requests received in last 5 minutes",
  "severity": 0,
  "enabled": true,
  "scopes": ["<app-insights-resource-id>"],
  "evaluationFrequency": "PT1M",
  "windowSize": "PT5M",
  "criteria": {
    "allOf": [{
      "query": "requests | where timestamp > ago(5m) | count",
      "timeAggregation": "Total",
      "dimensions": [],
      "operator": "LessThan",
      "threshold": 1,
      "failingPeriods": {
        "numberOfEvaluationPeriods": 1,
        "minFailingPeriodsToAlert": 1
      }
    }]
  },
  "actions": [{
    "actionGroupId": "<critical-action-group-id>"
  }]
}
```

#### High Error Rate

```json
{
  "name": "high-error-rate-critical",
  "description": "Error rate exceeds 5%",
  "severity": 0,
  "enabled": true,
  "evaluationFrequency": "PT1M",
  "windowSize": "PT5M",
  "criteria": {
    "allOf": [{
      "query": "requests | summarize ErrorRate = countif(success==false) * 100.0 / count()",
      "timeAggregation": "Average",
      "operator": "GreaterThan",
      "threshold": 5
    }]
  }
}
```

### Warning Alerts

#### Elevated Error Rate

```json
{
  "name": "elevated-error-rate",
  "description": "Error rate exceeds 1%",
  "severity": 2,
  "enabled": true,
  "evaluationFrequency": "PT5M",
  "windowSize": "PT15M",
  "criteria": {
    "allOf": [{
      "query": "requests | summarize ErrorRate = countif(success==false) * 100.0 / count()",
      "threshold": 1,
      "operator": "GreaterThan"
    }]
  }
}
```

#### High Response Time

```json
{
  "name": "high-response-time",
  "description": "P95 response time > 1000ms",
  "severity": 2,
  "enabled": true,
  "evaluationFrequency": "PT5M",
  "windowSize": "PT15M",
  "criteria": {
    "allOf": [{
      "query": "requests | summarize P95 = percentile(duration, 95)",
      "threshold": 1000,
      "operator": "GreaterThan"
    }]
  }
}
```

#### High CPU Usage

```json
{
  "name": "high-cpu-usage",
  "description": "CPU usage > 80%",
  "severity": 2,
  "enabled": true,
  "evaluationFrequency": "PT5M",
  "windowSize": "PT15M",
  "criteria": {
    "metricName": "Percentage CPU",
    "metricNamespace": "Microsoft.Web/sites",
    "operator": "GreaterThan",
    "threshold": 80,
    "timeAggregation": "Average"
  }
}
```

---

## 5. Action Groups

### Critical Action Group

```json
{
  "name": "critical-alerts",
  "shortName": "Critical",
  "enabled": true,
  "emailReceivers": [
    {
      "name": "OnCallTeam",
      "emailAddress": "oncall@example.com",
      "useCommonAlertSchema": true
    }
  ],
  "smsReceivers": [
    {
      "name": "OnCallPhone",
      "countryCode": "1",
      "phoneNumber": "1234567890"
    }
  ],
  "webhookReceivers": [
    {
      "name": "PagerDuty",
      "serviceUri": "https://events.pagerduty.com/integration/...",
      "useCommonAlertSchema": true
    }
  ]
}
```

### Warning Action Group

```json
{
  "name": "warning-alerts",
  "shortName": "Warning",
  "enabled": true,
  "emailReceivers": [
    {
      "name": "EngineeringTeam",
      "emailAddress": "engineering@example.com"
    }
  ],
  "webhookReceivers": [
    {
      "name": "Slack",
      "serviceUri": "https://hooks.slack.com/services/..."
    }
  ]
}
```

---

## 6. SLI/SLO Definitions

### Availability SLI/SLO

```kql
// Calculate Availability SLI
requests
| where timestamp > ago(30d)
| summarize 
    TotalRequests = count(),
    SuccessfulRequests = countif(success == true)
| extend AvailabilitySLI = SuccessfulRequests * 100.0 / TotalRequests
| extend SLO = 99.9
| extend ErrorBudget = (100 - SLO) * TotalRequests / 100
| extend ErrorBudgetRemaining = ErrorBudget - (TotalRequests - SuccessfulRequests)
| project 
    AvailabilitySLI,
    SLO,
    TotalRequests,
    SuccessfulRequests,
    FailedRequests = TotalRequests - SuccessfulRequests,
    ErrorBudget,
    ErrorBudgetRemaining
```

### Latency SLI/SLO

```kql
// Calculate Latency SLI
requests
| where timestamp > ago(30d)
| summarize 
    TotalRequests = count(),
    FastRequests = countif(duration < 200)
| extend LatencySLI = FastRequests * 100.0 / TotalRequests
| extend SLO = 95.0
| project LatencySLI, SLO, TotalRequests, FastRequests
```

---

## 7. Performance Baselines

### Establishing Baselines

```kql
// Calculate performance baselines from last 30 days
requests
| where timestamp > ago(30d)
| summarize 
    AvgDuration = avg(duration),
    P50Duration = percentile(duration, 50),
    P95Duration = percentile(duration, 95),
    P99Duration = percentile(duration, 99),
    AvgRequestRate = count() / 30.0 / 24.0 / 60.0,  // requests per minute
    AvgErrorRate = countif(success==false) * 100.0 / count()
| project 
    AvgDuration,
    P50Duration,
    P95Duration,
    P99Duration,
    AvgRequestRate,
    AvgErrorRate
```

---

**Template Version**: 1.0  
**Last Updated**: 2024-01-31

[Back to Stage 4 Overview](README.md)
