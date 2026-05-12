# Stock Portfolio Config Repo

Spring Cloud Config centralized configuration for all microservices.

## Structure
| File | Purpose |
|---|---|
| `application.properties` | Shared by all services: Eureka, actuator, JPA defaults |
| `application-dev.properties` | Dev overrides for all services |
| `application-shared-db.properties` | Local app/infra with shared EC2 MySQL |
| `application-prod.properties` | Prod overrides for EC2 hosts |
| `{service}.properties` | Service-specific common config: port, RabbitMQ keys, docs |
| `{service}-dev.properties` | Service DB URL to localhost |
| `{service}-shared-db.properties` | Service DB URL to EC2 MySQL while RabbitMQ, Redis, and Eureka stay local |
| `{service}-prod.properties` | Service DB URL to EC2 `18.61.227.114` |

## Profile Activation
Each service's local `application.yml`:

```yaml
spring:
  profiles:
    active: shared-db   # dev=local DB, shared-db=EC2 MySQL, prod=EC2 infra
  config:
    import: configserver:http://localhost:8888
```

## EC2 Databases
All 7 databases are expected on `18.61.227.114:3306`:

`auth_db`, `user_db`, `portfolio_db`, `price_db`, `alert_db`, `notif_db`, `report_db`

## Sensitive Values
Sensitive values come from the root `.env` and should not be committed:

`JWT_SECRET`, database passwords, `FINNHUB_API_KEY`, `MY_EMAIL_ID`, `MY_EMAIL_PASS`
