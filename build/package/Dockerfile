FROM golang:alpine3.12 as build

ARG VERSION
ENV K8S_VERSION=1.21
ENV K8S_SOURCE=https://github.com/kubernetes/kubernetes

# Add ca-certificates in the event final application needs ssl/tls
RUN apk add -q --no-cache alpine-sdk rsync bash ca-certificates
SHELL ["/bin/bash", "-c"]

RUN set -ex                                                  \
  && mkdir -p $GOPATH/src/k8s.io && cd $GOPATH/src/k8s.io    \
  && git clone --depth 1 -b release-$K8S_VERSION $K8S_SOURCE

RUN [[ -n "$( echo $VERSION | grep -v no-make )" ]] && ( cd $GOPATH/src/k8s.io/kubernetes/ && make all WHAT=cmd/kubectl GOFLAGS=-v ) || echo -e "\n\n-> ! Skipped k8s build ! <-\n\n"

###############################################################################
# final stage
FROM alpine:3.12
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=build /go/src/k8s.io/kubernetes/_output/local/go/bin/kubectl /kubectl

ENTRYPOINT ["/kubectl"]