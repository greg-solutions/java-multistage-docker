# Stage 1 (to create a "build" image, ~140MB)
FROM openjdk:8-jdk-alpine3.7 AS builder
RUN java -version

ARG APP_NAME=application-name
ARG APP_PATH=./build/libs/

COPY .. /build
WORKDIR /build

RUN ./gradlew clean build

COPY $APP_PATH/$APP_NAME-1.0-SNAPSHOT.jar /dist/java-application.jar

# Stage 2 (to create a downsized "container executable", ~87MB)
FROM openjdk:8u181-jre-alpine

COPY --from=builder /dist/java-application.jar ./opt/java-application.jar

WORKDIR /opt/$APP_NAME
## Wait for script (see https://github.com/ufoscout/docker-compose-wait/)

ADD https://github.com/ufoscout/docker-compose-wait/releases/download/2.5.0/wait /opt/java-application/wait
RUN chmod +x /opt/java-application/wait
CMD /opt/java-application/wait && java -jar /opt/java-application.jar