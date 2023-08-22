FROM ubuntu
#MAINTAINER Pythonlife
RUN apt update
RUN apt install -y openjdk-11-jre-headless
COPY target/mavendemo-0.0.1-SNAPSHOT.jar /opt/app.jar
WORKDIR /opt
CMD ["java", "-jar","app.jar"]
