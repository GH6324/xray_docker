# builder
FROM golang:alpine as builder
LABEL maintainer="wulabing <wulabing@gmail.com>"
LABEL version="0.0.23"


#ENV GOPROXY=https://goproxy.cn,direct
WORKDIR /app

RUN apk add --no-cache git  && git clone https://github.com/XTLS/Xray-core.git . && \
    go mod download && \
    go build -o xray /app/main/

# runner
FROM alpine:latest as runner


ENV UUID=""
ENV DEST=""
ENV SERVERNAMES=""
ENV PRIVATEKEY=""
ENV SHORTIDS=""
ENV NETWORK=""
ENV INTERNAL_PORT=""
ENV HOSTMODE_PORT=""
ENV TZ=Asia/Shanghai

WORKDIR /

COPY ./entrypoint.sh /
COPY ./config.json /

COPY --from=builder /app/xray /

RUN apk add --no-cache tzdata ca-certificates jq curl libqrencode-tools && \
    mkdir -p /var/log/xray &&\
    wget -O /geosite.dat https://github.com/v2fly/domain-list-community/releases/latest/download/dlc.dat && \
    wget -O /geoip.dat https://github.com/v2fly/geoip/releases/latest/download/geoip.dat && \
    chmod +x /entrypoint.sh


ENTRYPOINT ["./entrypoint.sh"]
EXPOSE 443
