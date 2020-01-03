FROM golang:1 AS builder
WORKDIR /go/src/github.com/demo/mockmetrics
COPY . .
RUN go get -v ./
RUN CGO_ENABLED=0 GOOS=linux go build -a -tags netgo -ldflags '-w'

# Final image.
FROM quay.io/prometheus/busybox:latest
COPY --from=builder /go/src/github.com/demo/mockmetrics /
EXPOSE 8080
CMD ["/mockmetrics"]
