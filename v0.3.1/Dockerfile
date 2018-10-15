FROM alpine:3.8

ENTRYPOINT ["/oklog"]

EXPOSE 7659 7651 7653 7650

RUN mkdir /data

ADD https://github.com/oklog/oklog/releases/download/v0.3.1/oklog-0.3.1-linux-amd64 /oklog
RUN ["chmod", "775", "/oklog"]

LABEL vendor=https://github.com/oklog/oklog \
	oklog.version=v0.3.1-linux-amd64 \
	oklog.release-date=2018-03-22
