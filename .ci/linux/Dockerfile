FROM golang:1.25
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
