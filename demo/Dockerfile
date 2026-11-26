# Spring Boot MCP server (payroll tools backend)
# Build & run from the demo/ directory:
#   docker build -t payroll-mcp-server .
#   docker run -p 8080:8080 --env-file .env payroll-mcp-server
#
# On Render: create a "Web Service" with root directory "demo" pointing at this
# Dockerfile, and set DB_HOST, DB_PORT, DB_NAME, DB_USER, DB_PASS, DB_SSLMODE and
# SECRET_KEY in the Render dashboard. SECRET_KEY MUST be the exact same value used
# by the FastAPI service, since it validates JWTs issued by FastAPI's /login.
# Render injects $PORT automatically; application.properties binds to it via
# server.port=${PORT:8080}.

# ---- Build stage ----
FROM maven:3.9.14-eclipse-temurin-17 AS build
WORKDIR /app

COPY pom.xml ./

# Warm the local Maven repo in its own layer so code changes don't re-download deps.
RUN mvn -B -ntp dependency:go-offline

COPY src ./src
RUN mvn -B -ntp clean package -DskipTests

# ---- Runtime stage ----
FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
