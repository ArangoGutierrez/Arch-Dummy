[![Docker Repository on Quay](https://quay.io/repository/eduardoarango/arch-dummy/status "Docker Repository on Quay")](https://quay.io/repository/eduardoarango/arch-dummy)
[![Go Report Card](https://goreportcard.com/badge/github.com/ArangoGutierrez/Arch-Dummy)](https://goreportcard.com/report/github.com/ArangoGutierrez/Arch-Dummy)

# Arch-Dummy

A dummy server that retrieves basic info from the host

# Build 

Clone the repo and change dir into the cloned folder

```bash
	CPU_ARCH=$(lscpu |grep -i 'model name'|awk '{print  "\""$3, $4, $5, $6, $7, $8"\""}') && \
	GIT_COMMIT=$(git rev-list -1 HEAD) && \
	BUILT=$(date) && \
	GO_VERSION=$(go version |awk '{print "\""$3, $4"\""}') && \	
	GO111MODULE=auto GOOS=linux go build -o dummy -mod=vendor \
	-ldflags "-X 'main.GitCommit=${GIT_COMMIT}' -X 'main.Built=${BUILT}' \
	-X 'main.GoVersion=${GO_VERSION}' \
	-X 'main.Arch=${CPU_ARCH}'" \
	/go/src/github.com/ArangoGutierrez/Arch-Dummy/cmd/dummy	
```

or using [buildah](https://buildah.io/)

```bash
GIT_COMMIT=$(git rev-list -1 HEAD)
buildah bud -t quay.io/<your-quay-user>/arch-dummy:$GIT_COMMIT -f build/Dockerfile
```


## Run 

To run simply 

```bash
./dummy
```

or from a container use [podman](https://podman.io/)

```bash
podman run --rm -p <any-port>:8080 quay.io/<your-quay-user>/arch-dummy
```

Now you can get the CPU info of the running host, along to the CPU of the host that built the image

```bash
[eduardo@fedora-ws Arch-Dummy]$ curl localhost:8080/version 
{Git Commit:"19b739ed40646cf2863a4175531c0bbd8cc02062",CPU_arch:"Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz",Built:"Thu Mar 26 22:35:00 UTC 2020",Go_version:"go1.12.8 linux/amd64"}
```

## API

`/version`	: will retrieve info of the build host

`/cpu`		: will retrieve info of the running host
