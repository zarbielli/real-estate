FROM --platform='linux/arm/v6' gobuffalo/buffalo:v0.18.7 as builder

ENV GOPROXY http://proxy.golang.org

RUN mkdir -p /src/study
WORKDIR /src/study

# Copy the Go Modules manifests
COPY ../../go.mod go.mod
COPY ../../go.sum go.sum

# Cache deps before building and copying source so that we don't need to re-download as much
# and so that source changes don't invalidate our downloaded layer
RUN go mod download

ADD ../.. .

ENV ADDR=0.0.0.0

EXPOSE 3000