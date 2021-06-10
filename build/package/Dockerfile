FROM golang:1.15.5-alpine3.12

ENV K8S_SOURCE=https://github.com/kubernetes/kubernetes

RUN set -ex                                      \
  && apk -q --no-cache add alpine-sdk bash rsync \
  && mkdir -p $GOPATH/src/k8s.io                 \
  && cd $GOPATH/src/k8s.io                       \
  && git clone --depth 1 $K8S_SOURCE             \
  && cd kubernetes/                              \
  && make all WHAT=cmd/kubectl GOFLAGS=-v
