# Kubectl for x86-64 and arm architectures
More precisely
* linux/amd64
* linux/arm/v6
* linux/arm/v7
* linux/arm64/v8

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