all:clear build

GIT_COMMIT := $(shell git rev-parse --verify HEAD --short=7)
BUILD_TIME := $(shell date -u +%Y%m%d.%H%M)
PACKAGES ?= $(shell go list ./... | grep -v /vendor/)
GOARCH ?= amd64

.PHONY: build
build:
	GOOS=linux GOARCH=${GOARCH} go build  -ldflags="-s -w -X 'main.Version=${GIT_COMMIT}' -X 'main.BuildTime=${BUILD_TIME}'" -v -o bin/nydus-overlayfs ./cmd/main.go

.PHONY: clear
clear:
	rm -f bin/*

.PHONY: static-release
static-release:
	GOOS=linux GOARCH=${GOARCH} go build -ldflags '-s -w -extldflags "-static"' -v -o bin/nydus-overlayfs ./cmd/main.go

.PHONY: test
test: build
	go vet $(PACKAGES)
	golangci-lint run
	go test -v -cover ${PACKAGES}
