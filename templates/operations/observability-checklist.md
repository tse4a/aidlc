# Observability Checklist

> **Team**: [Team Name]  
> **Feature**: [Feature Name]  
> **Service**: [Service Name]

---

## Overview

Ensure the new feature is properly instrumented for monitoring, debugging, and alerting.

---

## Metrics

### Required Metrics

| Metric | Type | Labels | Implemented |
|--------|------|--------|-------------|
| Request count | Counter | `method`, `status`, `endpoint` | ☐ |
| Request latency | Histogram | `method`, `endpoint` | ☐ |
| Error count | Counter | `method`, `error_type` | ☐ |
| [Feature-specific] | | | ☐ |

### Metric Implementation

```go
// Example: Counter
requestCount := otel.Meter("").Int64Counter(
    "feature_requests_total",
    metric.WithDescription("Total requests to feature"),
)

// Example: Histogram  
requestLatency := otel.Meter("").Float64Histogram(
    "feature_request_duration_seconds",
    metric.WithDescription("Request latency"),
)
```

### Cardinality Check

- [ ] Labels have bounded cardinality
- [ ] No user IDs or unbounded strings in labels
- [ ] Estimated unique label combinations: [X]

---

## Tracing

### Spans Created

| Operation | Span Name | Attributes | Implemented |
|-----------|-----------|------------|-------------|
| Handler entry | `[Service]/[Method]` | `request_id` | ☐ |
| Database query | `db.[operation]` | `db.statement` | ☐ |
| External call | `http.[service]` | `http.url` | ☐ |
| [Feature-specific] | | | ☐ |

### Span Implementation

```go
ctx, span := otel.Tracer("").Start(ctx, "OperationName",
    trace.WithAttributes(
        attribute.String("feature", "name"),
        attribute.String("request_id", requestID),
    ),
)
defer span.End()

// Record errors
if err != nil {
    span.RecordError(err)
    span.SetStatus(codes.Error, err.Error())
}
```

### Context Propagation

- [ ] Trace context passed through all layers
- [ ] Context propagated to async operations
- [ ] Context propagated to external calls

---

## Logging

### Log Points

| Event | Level | Fields | Implemented |
|-------|-------|--------|-------------|
| Request received | Info | `method`, `path`, `trace_id` | ☐ |
| Request completed | Info | `method`, `status`, `duration_ms` | ☐ |
| Error occurred | Error | `error`, `stack`, `trace_id` | ☐ |
| [Feature event] | Info | | ☐ |

### Log Implementation

```go
slog.InfoContext(ctx, "operation completed",
    "operation", "feature_action",
    "duration_ms", duration.Milliseconds(),
    "result", result,
)

slog.ErrorContext(ctx, "operation failed",
    "operation", "feature_action", 
    "error", err,
)
```

### Log Standards

- [ ] Using `slog` with structured fields
- [ ] Trace ID included via context
- [ ] No sensitive data (PII, secrets, tokens)
- [ ] Appropriate log levels used
- [ ] Error logs include stack traces

---

## Dashboards

### New Dashboard Panels

| Panel | Visualization | Query | Created |
|-------|---------------|-------|---------|
| Request Rate | Time series | `sum(rate(feature_requests_total[5m]))` | ☐ |
| Error Rate | Time series | `sum(rate(feature_requests_total{status="error"}[5m]))` | ☐ |
| Latency p50/p95/p99 | Time series | `histogram_quantile(0.99, ...)` | ☐ |
| [Feature-specific] | | | ☐ |

### Dashboard Location

- [ ] Dashboard created/updated: [Grafana URL]
- [ ] Dashboard added to team folder
- [ ] Dashboard variables configured (environment, service)

---

## Alerts

### New Alerts

| Alert | Condition | Severity | Runbook | Created |
|-------|-----------|----------|---------|---------|
| High Error Rate | error_rate > 5% for 5m | Critical | [link] | ☐ |
| High Latency | p99 > 500ms for 5m | Warning | [link] | ☐ |
| [Feature-specific] | | | | ☐ |

### Alert Configuration

```yaml
# Example Prometheus alert
- alert: FeatureHighErrorRate
  expr: |
    sum(rate(feature_requests_total{status="error"}[5m])) 
    / sum(rate(feature_requests_total[5m])) > 0.05
  for: 5m
  labels:
    severity: critical
    team: [team]
  annotations:
    summary: "High error rate on [feature]"
    runbook: "[runbook-url]"
```

### Alert Routing

- [ ] Alerts route to correct Slack channel
- [ ] On-call aware of new alerts
- [ ] Escalation path defined

---

## Runbooks

### Runbook Created

- [ ] Runbook location: [URL]
- [ ] Symptoms documented
- [ ] Diagnostic steps documented
- [ ] Remediation steps documented
- [ ] Escalation path documented

### Runbook Template

```markdown
# [Feature] Troubleshooting Runbook

## Symptoms
- [What does the alert/issue look like?]

## Impact
- [What is affected?]

## Diagnosis
1. Check [metric/log/trace]
2. Verify [component]
3. ...

## Remediation
1. [Immediate action]
2. [Follow-up action]

## Escalation
- Primary: [contact]
- Secondary: [contact]
```

---

## Verification

### Pre-Production

- [ ] Metrics visible in staging Grafana
- [ ] Traces visible in Tempo/Jaeger
- [ ] Logs visible in Loki/CloudWatch
- [ ] Alerts fire correctly (tested)

### Post-Production

- [ ] Metrics flowing in production
- [ ] Traces connecting across services
- [ ] Logs searchable and indexed
- [ ] Dashboards showing real data
- [ ] Alerts functioning

---

## References

- [Observability Guide](../../docs/dev/observability.md)
- [OpenTelemetry Deep-Dive](../../docs/opentelemetry.md)
- [Grafana Dashboards](grafana-url)
- [Alert Manager](alertmanager-url)
