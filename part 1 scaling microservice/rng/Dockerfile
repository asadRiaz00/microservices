#pull base image
FROM openjdk:8-jdk-alpine

#maintainer add your email id here 
MAINTAINER xyz@email.com

#expose port 8080 for ouside world
EXPOSE 8080

#default command to run jar
CMD java -jar /target/rng-0.0.1-SNAPSHOT.jar

#copy hello world to docker image
ADD ./target/rng-0.0.1-SNAPSHOT.jar /target/rng-0.0.1-SNAPSHOT.jar