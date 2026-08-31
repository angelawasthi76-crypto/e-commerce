FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app
COPY pom.xml .
COPY inventory-service/pom.xml inventory-service/
RUN --mount=type=cache,target=/root/.m2/repository \
    cd inventory-service && mvn dependency:go-offline -B
COPY inventory-service/src inventory-service/src
RUN cd inventory-service && mvn package -DskipTests -B

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/inventory-service/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
