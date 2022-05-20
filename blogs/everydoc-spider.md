# everydoc-spider

Everydoc-spider is a combine of everydoc and spider-flow, for beter performance.



## spider-flow

Dockerfile

```shell
#FROM java:8
FROM openjdk:8-jdk-alpine
#FROM openjdk:18-ea-11-jdk-alpine
MAINTAINER everydoc.icu

#RUN mkdir -p /spider-flow
#WORKDIR /spider-flow
EXPOSE 8088
ADD ./spider-flow-web/target/spider-flow.jar /root/app.jar


# add lux
ADD lib/lux_0.15.0_Linux_64-bit.tar.gz /usr/bin
# install ffmpeg for lux
RUN apk add --upgrade ffmpeg

# update alpine
RUN apk update
RUN apk add python py-pip curl unzip
RUN pip install selenium
RUN apk add chromium
RUN apk add libexif udev
RUN apk add chromium-chromedriver

#CMD sleep 30;java -Djava.security.egd=file:/dev/./urandom -jar spider-flow.jar
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "-Duser.timezone=GMT+8", "/root/app.jar"]




```

 

start spider-flow

```shell
docker run -t --name sf -p 8088:8088 imjcker/spider-flow:latest


```

