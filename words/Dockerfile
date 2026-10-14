FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline -q
COPY src/ src/
RUN mvn verify -q
RUN jlink \
    --add-modules java.base,java.logging,java.sql,java.naming,java.xml \
    --strip-debug \
    --no-man-pages \
    --no-header-files \
    --compress=2 \
    --output /jre-minimal
FROM alpine:3.19
ENV JAVA_HOME=/jre
ENV PATH="${JAVA_HOME}/bin:${PATH}"
COPY --from=builder /jre-minimal /jre
WORKDIR /app
COPY --from=builder /app/target/words.jar .
COPY --from=builder /app/target/dependency/ dependency/
EXPOSE 8080
CMD ["java", "-Xmx8m", "-Xms8m", "-jar", "words.jar"]