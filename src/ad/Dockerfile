# base image from dockerhub called as builder here
FROM eclipse-temurin:21-jdk AS builder

# For docker commands to run
WORKDIR /usr/src/app/

# Step 1: downloading dependencies 
# copy build/dependencies all required for building
COPY gradlew* settings.gradle* build.gradle .
COPY ./gradle ./gradle

# Provide execute permission on /gradlew to download dependencies
RUN chmod +x ./gradlew
RUN ./gradlew
RUN ./gradlew downloadRepos

# Below copy mean from local to within docker file
COPY . .
COPY ./pb ./proto
RUN chmod +x ./gradlew
# Proto here is called protocol buffers
RUN ./gradlew installDist -PprotoSourceDir=./proto

#####################################################
# Step 2: building takes place taking binary from Step 1 builder, above.
# Note: Java binary - is not self contained binaries 
FROM eclipse-temurin:21-jre

WORKDIR /usr/src/app/

COPY --from=builder /usr/src/app/ ./

ENV AD_PORT 9099

ENTRYPOINT ["./build/install/opentelemetry-demo-ad/bin/Ad"]

