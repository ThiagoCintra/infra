## multi-stage build: build with maven then run on lightweight JRE
FROM eclipse-temurin:21-jdk-alpine AS build
WORKDIR /workspace/app
COPY mvnw ./
COPY .mvn .mvn
COPY pom.xml ./
RUN ./mvnw dependency:go-offline -q
COPY src src
RUN ./mvnw -DskipTests package -q

FROM eclipse-temurin:21-jdk-alpine
WORKDIR /app
COPY --from=build /workspace/app/target/login-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8081
ENTRYPOINT ["java","-XX:+UseSerialGC","-jar","/app/app.jar"]
