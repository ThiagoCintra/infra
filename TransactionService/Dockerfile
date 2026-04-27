# Build stage
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /app
COPY proxy-ca.pem /tmp/proxy-ca.pem
RUN keytool -importcert -noprompt -trustcacerts \
    -keystore /opt/java/openjdk/lib/security/cacerts \
    -storepass changeit -file /tmp/proxy-ca.pem -alias proxy-ca
COPY pom.xml .
RUN mvn dependency:go-offline -B
COPY src ./src
RUN mvn clean package -DskipTests -B

# Runtime stage
FROM eclipse-temurin:17-jre
WORKDIR /app

RUN groupadd -r appgroup && useradd -r -g appgroup appuser
USER appuser

COPY --from=build /app/target/transaction-service-1.0.0.jar app.jar

ENV JAVA_OPTS="-Xms256m -Xmx512m -XX:+UseContainerSupport"

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=10s --start-period=60s --retries=3 \
  CMD curl -sf http://localhost:8080/actuator/health || exit 1

ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]
