FROM eclipse-temurin:21-jre
WORKDIR /app
COPY target/*.jar app.jar
# application.properties uses port 8081 for local runs; inside the container the API listens on 8080
ENV SERVER_PORT=8080
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
