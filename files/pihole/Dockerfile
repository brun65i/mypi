# The last alpine release dhcp-helper avalable in is 3.12.7
FROM alpine:3.12.7
RUN apk --no-cache add dhcp-helper
EXPOSE 67 67/udp
ENTRYPOINT ["dhcp-helper", "-n"]