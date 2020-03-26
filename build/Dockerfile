FROM registry.access.redhat.com/ubi8/go-toolset AS builder
WORKDIR /go/src/github.com/ArangoGutierrez/Arch-Dummy
COPY . .
RUN GO111MODULE=auto GOOS=linux go build -mod=vendor -o dummy /go/src/github.com/ArangoGutierrez/Arch-Dummy/cmd/dummy

FROM registry.access.redhat.com/ubi8/ubi-minimal
COPY --from=builder /go/src/github.com/ArangoGutierrez/Arch-Dummy/dummy /usr/bin/

ENTRYPOINT ["/usr/bin/dummy"]
LABEL io.k8s.display-name="CPU Dummy info server" \
      io.k8s.description="This is a light api that allow users to retrieve CPU arch info" 
