FROM registry.access.redhat.com/ubi8/go-toolset AS builder
WORKDIR /go/src/github.com/ArangoGutierrez/Arch-Dummy

USER root
COPY . .

RUN CPU_ARCH=$(lscpu |grep -i 'model name'|awk '{print  "\""$3, $4, $5, $6, $7, $8"\""}') && \
	GIT_COMMIT=$(git rev-list -1 HEAD) && \
	BUILT=$(date) && \
	GO_VERSION=$(go version |awk '{print "\""$3, $4"\""}') && \	
	GO111MODULE=auto GOOS=linux go build -o dummy -mod=vendor \
	-ldflags "-X 'main.GitCommit=${GIT_COMMIT}' -X 'main.Built=${BUILT}' \
	-X 'main.GoVersion=${GO_VERSION}' \
	-X 'main.Arch=${CPU_ARCH}'" \
	/go/src/github.com/ArangoGutierrez/Arch-Dummy/cmd/dummy

FROM registry.access.redhat.com/ubi8/ubi-minimal
COPY --from=builder /go/src/github.com/ArangoGutierrez/Arch-Dummy/dummy /usr/bin/

USER 1001

ENTRYPOINT ["/usr/bin/dummy"]
LABEL io.k8s.display-name="CPU Dummy info server" \
      io.k8s.description="This is a light api that allow users to retrieve CPU arch info" 
