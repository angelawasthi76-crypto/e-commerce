FROM eclipse-temurin:17-jdk-alpine AS build
WORKDIR /app
COPY pom.xml .
COPY user-service/pom.xml user-service/
RUN --mount=type=cache,target=/root/.m2/repository \
    cd user-service && mvn dependency:go-offline -B
COPY user-service/src user-service/src
RUN cd user-service && mvn package -DskipTests -B

FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/user-service/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
