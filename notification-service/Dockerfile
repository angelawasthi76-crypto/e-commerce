FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app
COPY pom.xml .
COPY notification-service/pom.xml notification-service/
RUN --mount=type=cache,target=/root/.m2/repository \
    cd notification-service && mvn dependency:go-offline -B
COPY notification-service/src notification-service/src
RUN cd notification-service && mvn package -DskipTests -B

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/notification-service/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
