# FROM openjdk:11.0.11-jdk

# WORKDIR /home

# COPY target/*.jar app.jar

# EXPOSE 8090

# ENTRYPOINT ["java","-jar","/home/app.jar"]

FROM maven:3.8.7-amazoncorretto-11 as build

COPY . .
RUN mvn clean package -DskipTests

FROM openjdk:11.0.11-jdk

WORKDIR /code
COPY --from=build /target/*.jar app.jar
EXPOSE 8090
ENTRYPOINT ["java","-jar","/code/app.jar"]