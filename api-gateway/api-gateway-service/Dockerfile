FROM golang:1.23.1 AS builder

ENV PROTOC_VERSION=23.3
ENV PROTOC_ZIP=protoc-${PROTOC_VERSION}-linux-x86_64.zip
RUN apt-get update && apt-get install -y unzip
RUN curl -OL https://github.com/protocolbuffers/protobuf/releases/download/v${PROTOC_VERSION}/$PROTOC_ZIP \
    && unzip -o $PROTOC_ZIP -d /usr/local bin/protoc \
    && unzip -o $PROTOC_ZIP -d /usr/local 'include/*' \
    && rm -f $PROTOC_ZIP
RUN go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
RUN go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
RUN export PATH="$PATH:$(go env GOPATH)/bin"

RUN go install github.com/swaggo/swag/cmd/swag@latest

WORKDIR /usr/src/app
COPY . .
RUN protoc --proto_path=proto --go_out=proto --go-grpc_out=proto proto/*.proto

RUN go mod tidy
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o ./exe/main ./cmd/main.go

FROM golang:1.23.1-alpine
COPY --from=builder /usr/src/app/exe .
ENTRYPOINT ["./main"]