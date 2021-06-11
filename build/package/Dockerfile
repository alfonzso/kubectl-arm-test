ARG KUBECTL_BIN_PATH=/go/src/k8s.io/kubernetes/_output/local/go/bin

FROM golang:alpine3.12 as build

ARG VERSION
ARG KUBECTL_BIN_PATH
ENV K8S_VERSION=1.21
ENV K8S_TAG='v1.21.2-rc.0-65-g010de9b'
ENV K8S_SOURCE=https://github.com/kubernetes/kubernetes
ENV GO_HELLO_WORLD='package main; import "fmt"; func main() { fmt.Println("hello world") }'

# Add ca-certificates in the event final application needs ssl/tls
RUN apk add -q --no-cache alpine-sdk rsync bash ca-certificates
SHELL ["/bin/bash", "-c"]

# because of git clone --depth 1, we need run "git tag -a '' -m ''" manually on current branch
# this needed by kubernetes build ( git describe ),
# at the end this string become "GitVersion:v1.21.2-rc.0-65-g010de9b" if 'kubectl version' command issued
#
# without this, you will get something like this: GitVersion:"v0.0.0-master+$Format:%H$",
# and error like: could not parse pre-release/metadata (-master+$Format:%H$) in version "v0.0.0-master+$Format:%H$"

RUN set -ex                                                   \
  && mkdir -p $GOPATH/src/k8s.io && cd $GOPATH/src/k8s.io     \
  && git clone --depth 1 -b release-$K8S_VERSION $K8S_SOURCE  \
  && cd kubernetes                                            \
  && git config --global user.email "tst@tst.tst"             \
  && git config --global user.name "Tst Tst"                  \
  && git tag -a "${K8S_TAG}" -m "${K8S_TAG}"                  \
  && git describe

RUN [[ -n "$( echo $VERSION | grep -v no-make )" ]]  \
  && cd $GOPATH/src/k8s.io/kubernetes/               \
  && make all WHAT=cmd/kubectl GOFLAGS=-v            \
  || echo -e "\n\n-> ! Skipped k8s build ! <-\n\n"

RUN [[ -n "$( echo $VERSION | grep no-make )" ]]            \
  && mkdir -p ${KUBECTL_BIN_PATH} && cd ${KUBECTL_BIN_PATH} \
  && echo $GO_HELLO_WORLD                                   \
     | tee ${KUBECTL_BIN_PATH}/kubectl.go                   \
     | go build ${KUBECTL_BIN_PATH}/kubectl.go              \
  || echo -e "\n\n-> ! Skipped go hello-world build ! <-\n\n"

###############################################################################
# final stage
FROM alpine:3.12
ARG KUBECTL_BIN_PATH
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=build ${KUBECTL_BIN_PATH}/kubectl /kubectl

ENTRYPOINT ["/kubectl"]