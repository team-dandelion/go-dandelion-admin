FROM golang:1.20 as builder

ENV GO111MODULE=on \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64 \
    GOPROXY=https://goproxy.cn,direct

WORKDIR /go-admin

COPY . .

RUN go mod tidy && go build -a -o gateway ./gateway

FROM alpine as final

ENV DANDELION_ENV="production"

WORKDIR /opt/app

COPY --from=builder /go-admin/gateway/gateway .
COPY --from=builder /go-admin/gateway/config ./config
EXPOSE 8999
CMD ["/opt/app/gateway", "server"]
