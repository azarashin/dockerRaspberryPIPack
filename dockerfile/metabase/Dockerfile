FROM debian

RUN apt update
RUN apt upgrade
RUN apt install -y wget
RUN apt install -y default-jdk-headless
RUN wget -O metabase.jar http://downloads.metabase.com/v0.32.8/metabase.jar
CMD java -jar metabase.jar

