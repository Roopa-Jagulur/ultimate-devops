# This is multi stage dockerfile - better practice as DevSecOps Practice which only copies the binary onto the second stage.
# At first stage, creating product-catalog binary
# golang base image here called as builder
FROM golang:1.22-alpine AS builder

# Where your docker commands are executed
WORKDIR /usr/src/app/

# Use Go build cache for dependencies
RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build

# Copy
COPY go.mod go.sum ./

# Step 1
# Download
RUN go mod download

# Step 2
# Copy the rest of the source code
COPY . .

# Step 3
RUN go build -o product-catalog .

####################################
# Second stage contain less number of binaries and it is light weight image - when this is the case, chance of vulnerabilities will significantly go down.
# This image will have basic system level dependencies, no application dependencies and you will copy the binary from first stage above and execute it here.
# This is the stage which is executed as container.
FROM alpine AS release

WORKDIR /usr/src/app/

# Below line mean copy ./Product into docker image ./products
COPY ./products/ ./products/
COPY --from=builder /usr/src/app/product-catalog/ ./

# Exposing port
ENV PRODUCT_CATALOG_PORT=8088
# Running product-catalog binary
ENTRYPOINT [ "./product-catalog" ]

# Note: goland binary is called as self contained binaries - means you can take the go binary or the go executable and run it on the machine, even where go is not installed.
