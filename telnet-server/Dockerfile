# Build stage
# Pulls in parent image named 'golang:alpine'
FROM golang:alpine AS build-env
# copies all Go source code in the current local directory
ADD . /
# navigate to the root directory in the image filesystem
# uses go build to build the Go binary named telnet-server
RUN cd / && go build -o telnet-server

# final stage
# pull again with no dependencies
FROM alpine:latest as final
# sets the working directory for the application
WORKDIR /app
# set env variables in the container image that the application can use:
# set telnet-server to port 2323
# metric server on port 9000
ENV TELNET_PORT 2323
ENV METRIC_PORT 9000
# copy the telnet-server Goland binary from the build-env to the final env
COPY --from=build-env /telnet-server /app/

# invokes the telnet-server binary when the container starts to execute 
# the sample application. ENTRYPOINT is used instead of CMD becase 
# additional flags will be required for the container test later. 
ENTRYPOINT ["./telnet-server"]

