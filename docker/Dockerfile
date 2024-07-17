FROM openjdk:17-jdk-slim

WORKDIR /minecraft

RUN apt-get update && apt-get install -y wget && \
    wget https://papermc.io/api/v2/projects/paper/versions/1.19.4/builds/497/downloads/paper-1.19.4-497.jar -O paper.jar

COPY server.properties .

EXPOSE 25565

CMD ["java", "-Xmx1G", "-Xms1G", "-jar", "paper.jar", "nogui"]