# tezos-opam-builder
This docker images are used for building the Tezos Opam Builder, which is used for building TezEdge Ffi for Tezos in fork:
https://gitlab.com/tezedge/tezos.git

Images are automatically build https://hub.docker.com/repository/docker/tezedge/tezos-opam-builder


### TODO: manual build/push
ubuntu 21 build requires docker version at least 20.10.X, but dockerhub has just 19.X, so we upload this two images manually


#### Local build/push:
```
...
docker login docker.io

docker build -t tezedge/tezos-opam-builder:ubuntu21 -f ubuntu/21.04_hirsute/Dockerfile .
docker push tezedge/tezos-opam-builder:ubuntu21

docker build -t tezedge/tezos-opam-builder:opensuse_tumbleweed -f opensuse/tumbleweed/Dockerfile .
docker push tezedge/tezos-opam-builder:opensuse_tumbleweed

...
```