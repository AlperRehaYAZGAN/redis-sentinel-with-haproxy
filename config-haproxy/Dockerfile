FROM haproxy:2.6.2-alpine3.16

COPY haproxy.cfg .

RUN mkdir -p /etc/haproxy &&\
    cp haproxy.cfg /etc/haproxy/haproxy.cfg

CMD [ "haproxy", "-f", "/etc/haproxy/haproxy.cfg" ]