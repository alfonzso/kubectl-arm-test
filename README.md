# Kubectl for x86-64 and arm architectures
More precisely
* linux/amd64
* linux/arm/v6
* linux/arm/v7
* linux/arm64/v8

## About kubectl
Source code pulled from here ( with this command ) :
```shell
git clone --depth 1 -b release-1.21 https://github.com/kubernetes/kubernetes
```
So kubectl version will be 1.21+ <br>
For more information please read Dockerfiles [here](https://github.com/alfonzso/kubectl-mutli-arch/tree/master/build/package).

## Usage
### Test the binary only, and print kubectl version
```
docker run alfonzso/kubectl-mutli-arch:1.0.2 version
```

### Use with your existing .kube/config file
```
# if you are in your home, beside .kube directory
docker run -e KUBECONFIG=/kube/config -v $(pwd)/.kube/config:/kube/config alfonzso/kubectl-mutli-arch:1.0.2 get po -A
```

## Dockerhub
https://hub.docker.com/r/alfonzso/kubectl-mutli-arch

Please don't use any image with "-no-make" tag, its only for testing and print "hello world"

# Current stable version: alfonzso/kubectl-mutli-arch:1.0.2