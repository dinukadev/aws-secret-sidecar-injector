FROM amazonlinux AS build
RUN yum -y update && yum -y install tar gzip
RUN curl -o go1.14.3.linux-amd64.tar.gz https://dl.google.com/go/go1.14.3.linux-amd64.tar.gz -s
RUN tar -C /usr/local -xzf go1.14.3.linux-amd64.tar.gz
ENV PATH="/usr/local/go/bin:${PATH}"
WORKDIR /src
COPY ./go.mod /src/go.mod
RUN go mod download
COPY . /src
RUN go build -o /app -v

FROM amazonlinux:latest
RUN yum -y update && yum install -y ca-certificates && rm -rf /var/cache/yum/*
COPY --from=build /app /app
ENTRYPOINT ["/app"]
