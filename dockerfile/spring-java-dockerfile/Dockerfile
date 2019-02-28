FROM maven:3-jdk-8-alpine as builder
ARG MAIN_CLASS
ARG BASE_PACKAGE=src/main/java

WORKDIR /app
ADD pom.xml .
COPY $BASE_PACKAGE/$MAIN_CLASS $BASE_PACKAGE/$MAIN_CLASS
# Hacky :P
RUN mvn -U clean package -Dmaven.test.skip=true
ADD src src
RUN mvn -U package -Dmaven.test.skip=true

FROM openjdk:8-alpine
ARG JAR_FILE
HEALTHCHECK --interval=5s --timeout=2s CMD wget -qO- http://localhost:8080/actuator/health || exit 1
WORKDIR /app
COPY --from=builder /app/target/$JAR_FILE $JAR_FILE
RUN ls -al
ENV ENV_JAR_FILE ${JAR_FILE}
CMD java -jar $ENV_JAR_FILE