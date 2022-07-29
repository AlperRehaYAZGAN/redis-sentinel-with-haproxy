FROM haproxy:2.6.2-alpine

USER root
RUN mkdir -p /etc/haproxy

COPY haproxy.cfg /etc/haproxy/haproxy.cfg

USER haproxy

CMD [ "haproxy", "-f", "/etc/haproxy/haproxy.cfg" ]