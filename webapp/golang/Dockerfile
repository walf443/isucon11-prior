FROM golang:1.16 as builder

WORKDIR /go/src

COPY go.mod go.sum ./
RUN go mod download

COPY ./*.go  ./

ARG CGO_ENABLED=0
ARG GOOS=linux
ARG GOARCH=amd64
RUN go build \
    -o /go/bin/main \
    -ldflags '-s -w'

FROM jwilder/dockerize as dockerize

FROM alpine as runner

COPY --from=dockerize /usr/local/bin/dockerize /
COPY --from=builder /go/bin/main /app/main

ENV DB_HOST localhost

ENTRYPOINT ["/app/main"]

