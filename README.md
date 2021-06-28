# tezos-opam-builder
This docker images are used for building the Tezos Opam Builder, which is used for building TezEdge Ffi for Tezos in fork:
https://gitlab.com/tezedge/tezos.git

Images are automatically build https://hub.docker.com/repository/docker/tezedge/tezos-opam-builder

#### Local build:
```
...
docker build -f ubuntu/21.04_hirsute/Dockerfile .
...
```