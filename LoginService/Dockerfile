## multi-stage build: build with maven then run on lightweight JRE
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /workspace/app
COPY proxy-ca.pem /tmp/proxy-ca.pem
RUN keytool -importcert -noprompt -trustcacerts \
    -keystore /opt/java/openjdk/lib/security/cacerts \
    -storepass changeit -file /tmp/proxy-ca.pem -alias proxy-ca
COPY pom.xml ./
RUN mvn dependency:go-offline -q
COPY src src
RUN mvn -DskipTests package -q

FROM eclipse-temurin:21-jdk
WORKDIR /app
COPY --from=build /workspace/app/target/login-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8081
ENTRYPOINT ["java","-XX:+UseSerialGC","-jar","/app/app.jar"]
