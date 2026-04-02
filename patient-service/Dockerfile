FROM eclipse-temurin:21-jdk

WORKDIR /app

# Copy jar from local machine
COPY target/*.jar app.jar

EXPOSE 4000

ENTRYPOINT ["java", "-jar", "app.jar"]