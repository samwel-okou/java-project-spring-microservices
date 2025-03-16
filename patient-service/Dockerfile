#FROM maven:3.9.9-eclipse-temurin-21 AS builder
#
#WORKDIR /app
#
#COPY pom.xml .
#
#RUN mvn dependency:go-offline -B
#
#COPY src ./src
#
#RUN mvn clean package
#
#FROM openjdk:21-jdk AS runner
#
#WORKDIR /app
#
#COPY --from=builder ./app/target/patient-service-0.0.1-SNAPSHOT.jar ./app.jar
#
#EXPOSE 4000
#
#ENTRYPOINT ["java", "-jar", "app.jar"]
# Stage 1: Build Stage
FROM maven:3.9.9-eclipse-temurin-21 AS builder

WORKDIR /app

# Copy only the necessary files first to leverage Docker layer caching
COPY pom.xml .

# Download dependencies only (cache-friendly)
RUN mvn dependency:go-offline -B

# Copy source files
COPY src ./src

# Package application (skip tests for faster build)
RUN mvn clean package -DskipTests

# Stage 2: Runtime Stage
FROM openjdk:21-jdk AS runner

WORKDIR /app

# Copy built JAR from builder stage
COPY --from=builder /app/target/patient-service-0.0.1-SNAPSHOT.jar ./app.jar

# Expose application port
EXPOSE 4000

# Run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
