# Stock Portfolio Config Repo

Spring Cloud Config — centralized configuration for all microservices.

## Structure
| File | Purpose |
|---|---|
| `application.properties` | Shared by ALL services (Eureka, actuator, JPA defaults) |
| `application-dev.properties` | Dev overrides for all services |
| `application-prod.properties` | Prod overrides (EC2 hosts) for all services |
| `{service}.properties` | Service-specific common config (port, RabbitMQ keys, etc.) |
| `{service}-dev.properties` | Service DB URL → localhost |
| `{service}-prod.properties` | Service DB URL → EC2 18.61.227.114 |

## Profile Activation
Each service's local `application.yml`:
```yaml
spring:
  profiles:
    active: dev   # change to prod for EC2
  config:
    import: optional:configserver:http://localhost:8888
```

## EC2 Databases (prod)
All 7 databases on `18.61.227.114:3306`
auth_db | user_db | portfolio_db | price_db | alert_db | notif_db | report_db

## Sensitive Values (from root .env — never committed)
JWT_SECRET, AUTH_DB_PASSWORD, USER_DB_PASSWORD, PORTFOLIO_DB_PASSWORD,
PRICE_DB_PASSWORD, ALERT_DB_PASSWORD, NOTIF_DB_PASSWORD, REPORT_DB_PASSWORD,
FINNHUB_API_KEY, MY_EMAIL_ID, MY_EMAIL_PASS
