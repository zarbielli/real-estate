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
RUN buffalo build --static -o /bin/app

FROM --platform='linux/arm/v6' golang:alpine3.15  as dev
RUN apk add --no-cache bash
RUN apk add --no-cache ca-certificates

WORKDIR /bin/

COPY --from=builder /bin/app .

# Uncomment to run the binary in "production" mode:
# ENV GO_ENV=production

# Bind the app to 0.0.0.0 so it can be seen from outside the container
ENV ADDR=0.0.0.0

EXPOSE 3000

# Uncomment to run the migrations before running the binary:
# CMD /bin/app migrate; /bin/app
CMD exec /bin/app
