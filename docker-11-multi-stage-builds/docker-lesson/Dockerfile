# ---- build stage (JDK)
FROM eclipse-temurin:11-jdk-jammy AS builder
WORKDIR /app
COPY . .
RUN javac App.java

# ---- runtime stage (JRE)
FROM eclipse-temurin:11-jre-jammy
WORKDIR /myapp
COPY --from=builder /app/App.class .
CMD ["java", "App"]