FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app
COPY pom.xml .
COPY api-gateway/pom.xml api-gateway/
RUN --mount=type=cache,target=/root/.m2/repository \
    cd api-gateway && mvn dependency:go-offline -B
COPY api-gateway/src api-gateway/src
RUN cd api-gateway && mvn package -DskipTests -B

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/api-gateway/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
