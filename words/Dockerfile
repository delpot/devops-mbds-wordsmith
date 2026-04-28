FROM maven:3.9.15-eclipse-temurin-21 AS builder
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src src
RUN mvn verify

FROM eclipse-temurin:21-jdk-alpine AS jre-builder
RUN $JAVA_HOME/bin/jlink \
        --add-modules java.base,java.sql,jdk.httpserver \
        --strip-debug \
        --no-man-pages \
        --no-header-files \
        --compress=2 \
        --output /javaruntime

FROM alpine:latest
ENV JAVA_HOME=/opt/java/openjdk/
ENV PATH="${JAVA_HOME}/bin:${PATH}"
COPY --from=jre-builder /javaruntime $JAVA_HOME
RUN mkdir /opt/app
COPY --from=builder target /opt/app/target
CMD ["java", "-Xmx8m", "-Xms8m", "-jar", "/opt/app/target/words.jar"]