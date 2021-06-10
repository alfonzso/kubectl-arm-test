FROM golang:1.15.5-alpine3.12

RUN apk add --update alpine-sdk bash rsync             \
    mkdir -p $GOPATH/src/k8s.io                        \
    cd $GOPATH/src/k8s.io                              \
    git clone https://github.com/kubernetes/kubernetes \
    cd kubernetes/                                     \
    make all WHAT=cmd/kubectl GOFLAGS=-v