# Use fully qualified names to avoid Podman "short-name" errors
FROM docker.io/library/maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /opt/server
COPY pom.xml .
COPY src ./src
RUN mvn clean package && \
    mv target/shipping-*.jar shipping.jar

# Use fully qualified names here as well
FROM docker.io/library/eclipse-temurin:17-jre-alpine
EXPOSE 8080
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
WORKDIR /opt/server
ENV CART_ENDPOINT="cart:8080" \
    DB_HOST="mysql"
COPY --from=builder --chown=roboshop:roboshop /opt/server/shipping.jar .
USER roboshop
ENTRYPOINT ["sh", "-c", "sleep 30 && java -jar shipping.jar"]