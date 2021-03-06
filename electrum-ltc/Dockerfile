FROM python:3.7-alpine

ARG VERSION

WORKDIR /root

COPY checksum.sha256 .

ENV ELECTRUM_VERSION $VERSION
ENV ELECTRUM_USER electrum
ENV ELECTRUM_HOME /home/$ELECTRUM_USER

RUN set -x && apk --update-cache add --virtual build-dependencies gcc musl-dev openssl-dev && \
	adduser -D $ELECTRUM_USER && \
	wget -q https://github.com/pooler/electrum-ltc/archive/${ELECTRUM_VERSION}.tar.gz && \
	cat checksum.sha256 | grep ${VERSION} | sha256sum -c && \
	pip3 install scrypt && \
	pip3 install ${ELECTRUM_VERSION}.tar.gz && \
	apk del build-dependencies

RUN mkdir -p ${ELECTRUM_HOME}/.electrum/ /data && \
	ln -sf ${ELECTRUM_HOME}/.electrum/ /data && \
	chown ${ELECTRUM_USER} ${ELECTRUM_HOME}/.electrum /data

USER $ELECTRUM_USER
WORKDIR $ELECTRUM_HOME
VOLUME /data
EXPOSE 7000

COPY start-electrum-ltc.py /usr/local/bin/
CMD ["/usr/local/bin/start-electrum-ltc.py"]
