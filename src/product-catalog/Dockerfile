# Use Golang base image (1.22) with Alpine for the build stage
FROM golang:1.22-alpine AS builder

# Set the working directory in the container to /usr/src/app/
WORKDIR /usr/src/app/

# Copy all the source code from the host machine into the container’s working directory
COPY . .

# Download all the Go dependencies specified in go.mod file
RUN go mod download

# Build the Go application and output the binary as 'product-catalog'
RUN go build -o product-catalog ./

####################################

# Use a minimal Alpine Linux image for the release stage
FROM alpine AS release

# Set the working directory in the container for the release stage to /usr/src/app/
WORKDIR /usr/src/app/

# Copy the 'products/' directory from the host machine into the container's working directory
COPY ./products/ ./products/

# Copy the 'product-catalog' executable built in the 'builder' stage into the release stage
COPY --from=builder /usr/src/app/product-catalog/ ./

# Set the environment variable 'PRODUCT_CATALOG_PORT' to 8088 (used by the app to listen on port 8088)
ENV PRODUCT_CATALOG_PORT 8088

# Set the entry point for the container, the application will run the 'product-catalog' executable
ENTRYPOINT [ "./product-catalog" ]
