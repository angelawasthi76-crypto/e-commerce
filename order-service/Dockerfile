FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app
COPY pom.xml .
COPY order-service/pom.xml order-service/
RUN --mount=type=cache,target=/root/.m2/repository \
    cd order-service && mvn dependency:go-offline -B
COPY order-service/src order-service/src
RUN cd order-service && mvn package -DskipTests -B

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/order-service/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
