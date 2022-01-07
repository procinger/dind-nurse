# build environment ###########################################
FROM golang:1.17.3-alpine@sha256:a207b29286084e7342286de809756f61558b00b81f794406399027631e0dba8b AS build-env

WORKDIR /app

# entrypoint
RUN apk add --no-cache entr docker
COPY ./deployment/entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

# dependencies
COPY go.mod .
COPY go.sum .
RUN go mod download

# server
COPY main.go .
COPY pkg ./pkg
RUN go install .

# production image ############################################
FROM alpine:3.15.0@sha256:21a3deaa0d32a8057914f36584b5288d2e5ecc984380bc0118285c70fa8c9300 AS prod
RUN apk add --no-cache ca-certificates

# executable
COPY --from=build-env /go/bin/dind-nurse /bin/dind-nurse
ENTRYPOINT ["dind-nurse"]
