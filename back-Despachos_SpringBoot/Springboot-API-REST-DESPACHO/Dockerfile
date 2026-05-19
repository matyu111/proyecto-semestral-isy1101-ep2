# Etapa 1: Construcción del backend con Maven
FROM maven:3.9.6-eclipse-temurin-17 AS build

WORKDIR /app

COPY pom.xml .

RUN mvn dependency:go-offline -B

COPY src ./src

RUN mvn clean package -DskipTests


# Etapa 2: Ejecución segura del microservicio
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=build /app/target/*.jar app.jar

USER appuser

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]
