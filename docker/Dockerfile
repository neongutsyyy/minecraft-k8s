FROM openjdk:21-slim

WORKDIR /minecraft

RUN apt-get update && apt-get install -y wget && \
    wget https://api.papermc.io/v2/projects/paper/versions/1.20.6/builds/148/downloads/paper-1.20.6-148.jar -O paper.jar

# Create eula.txt and accept EULA
RUN echo "eula=true" > eula.txt

COPY server.properties .

EXPOSE 25565

CMD ["java", "-Xmx1G", "-Xms1G", "-jar", "paper.jar", "nogui"]