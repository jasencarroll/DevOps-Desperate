```bash
jasen@Renegade:devOps(main*)$ minikube start --driver=virtualbox
W0315 21:40:31.503000   12953 main.go:291] Unable to resolve the current Docker CLI context "default": context "default" does not exist
😄  minikube v1.29.0 on Ubuntu 22.04
✨  Using the virtualbox driver based on user configuration
👍  Starting control plane node minikube in cluster minikube
🔥  Creating virtualbox VM (CPUs=2, Memory=6000MB, Disk=20000MB) ...
🐳  Preparing Kubernetes v1.26.1 on Docker 20.10.23 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🔎  Verifying Kubernetes components...
🌟  Enabled addons: default-storageclass, storage-provisioner
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
jasen@Renegade:devOps(main*)$ minikube docker-env
W0315 21:42:09.517386   13977 main.go:291] Unable to resolve the current Docker CLI context "default": context "default" does not exist
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.59.106:2376"
export DOCKER_CERT_PATH="/home/jasen/.minikube/certs"
export MINIKUBE_ACTIVE_DOCKERD="minikube"

# To point your shell to minikube's docker-daemon, run:
# eval $(minikube -p minikube docker-env)
jasen@Renegade:devOps(main*)$ ^C
jasen@Renegade:devOps(main*)$ eval $(minikube -p minikube docker-env)
W0315 21:42:35.665405   14080 main.go:291] Unable to resolve the current Docker CLI context "default": context "default" does not exist
jasen@Renegade:devOps(main*)$ docker --version
Docker version 23.0.1, build a5ee5b1
jasen@Renegade:devOps(main*)$ cd telnet-server/
jasen@Renegade:telnet-server(main*)$ docker build -t dftd/telnet-server:v1 .
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  30.72kB
Step 1/9 : FROM golang:alpine AS build-env
alpine: Pulling from library/golang
63b65145d645: Pull complete 
a2d21d5440eb: Pull complete 
935e6c44a52c: Pull complete 
94cc34f8dd06: Pull complete 
Digest: sha256:1db127655b32aa559e32ed3754ed2ea735204d967a433e4b605aed1dd44c5084
Status: Downloaded newer image for golang:alpine
 ---> 898000b2160b
Step 2/9 : ADD . /
 ---> 7cc76dc4198d
Step 3/9 : RUN cd / && go build -o telnet-server
 ---> Running in 72e12a126ee6
go: downloading github.com/prometheus/client_golang v1.6.0
go: downloading github.com/beorn7/perks v1.0.1
go: downloading github.com/cespare/xxhash/v2 v2.1.1
go: downloading github.com/golang/protobuf v1.4.0
go: downloading github.com/prometheus/common v0.9.1
go: downloading github.com/prometheus/client_model v0.2.0
go: downloading github.com/prometheus/procfs v0.0.11
go: downloading google.golang.org/protobuf v1.21.0
go: downloading github.com/matttproud/golang_protobuf_extensions v1.0.1
go: downloading golang.org/x/sys v0.0.0-20200420163511-1957bb5e6d1f
Removing intermediate container 72e12a126ee6
 ---> d8883e3d47b3
Step 4/9 : FROM alpine:latest as final
latest: Pulling from library/alpine
63b65145d645: Already exists 
Digest: sha256:ff6bdca1701f3a8a67e328815ff2346b0e4067d32ec36b7992c1fdc001dc8517
Status: Downloaded newer image for alpine:latest
 ---> b2aa39c304c2
Step 5/9 : WORKDIR /app
 ---> Running in 78924180d9c7
Removing intermediate container 78924180d9c7
 ---> 4827fa0a2a99
Step 6/9 : ENV TELNET_PORT 2323
 ---> Running in 19483d0b9c7f
Removing intermediate container 19483d0b9c7f
 ---> 207a91df947d
Step 7/9 : ENV METRIC_PORT 9000
 ---> Running in 86b0e6a6e037
Removing intermediate container 86b0e6a6e037
 ---> e4d674d473da
Step 8/9 : COPY --from=build-env /telnet-server /app/
 ---> 862c1462ab4a
Step 9/9 : ENTRYPOINT ["./telnet-server"]
 ---> Running in 22fffb844758
Removing intermediate container 22fffb844758
 ---> 2f32fb902fc6
Successfully built 2f32fb902fc6
Successfully tagged dftd/telnet-server:v1
jasen@Renegade:telnet-server(main*)$ docker image ls dftd/telnet-server:v1
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
dftd/telnet-server   v1        2f32fb902fc6   10 seconds ago   19MB
jasen@Renegade:telnet-server(main*)$ docker run -p 2323:2323 -d --name telnet-server dftd/telnet-server:v1
507cda205191e7f9afd9a68d650e6c5c9e54c5a6ae09abfdfa6782b179bf6607
jasen@Renegade:telnet-server(main*)$ docker container ls -f name=telnet-server
CONTAINER ID   IMAGE                   COMMAND             CREATED          STATUS         PORTS                    NAMES
507cda205191   dftd/telnet-server:v1   "./telnet-server"   10 seconds ago   Up 9 seconds   0.0.0.0:2323->2323/tcp   telnet-server
jasen@Renegade:telnet-server(main*)$ minikube ip
192.168.59.106
jasen@Renegade:telnet-server(main*)$ telnet 192.168.59.106 2323
Trying 192.168.59.106...
Connected to 192.168.59.106.
Escape character is '^]'.

____________ ___________
|  _  \  ___|_   _|  _  \
| | | | |_    | | | | | |
| | | |  _|   | | | | | |
| |/ /| |     | | | |/ /
|___/ \_|     \_/ |___/

>d
Thu Mar 16 04:45:47 +0000 UTC 2023
>q
Good Bye!
Connection closed by foreign host.
```bash