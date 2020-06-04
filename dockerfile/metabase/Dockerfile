FROM debian

RUN apt update
RUN apt upgrade
RUN apt install -y wget
RUN apt install -y default-jdk-headless
RUN mkdir /metabase
RUN wget -O /metabase/metabase.jar http://downloads.metabase.com/v0.32.8/metabase.jar
##RUN wget -O /metabase.jar http://downloads.metabase.com/v0.32.8/metabase.jar

VOLUME /metabase

EXPOSE 3000
WORKDIR /metabase/
#WORKDIR /
ENTRYPOINT ["java", "-jar", "metabase.jar"]
#ENTRYPOINT ["/bin/bash"]

