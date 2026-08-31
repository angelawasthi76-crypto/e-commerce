FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app
COPY pom.xml .
COPY product-service/pom.xml product-service/
RUN --mount=type=cache,target=/root/.m2/repository \
    cd product-service && mvn dependency:go-offline -B
COPY product-service/src product-service/src
RUN cd product-service && mvn package -DskipTests -B

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/product-service/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
