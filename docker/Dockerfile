FROM pihole/pihole:v5.8.1

RUN apt-get update && apt-get install -Vy php-cli php-sqlite3 php-intl php-curl

RUN wget -O - https://raw.githubusercontent.com/jacklul/pihole-updatelists/master/install.sh | bash

RUN apt update && apt install -y unbound

RUN echo "include: \"/opt/unbound/*.conf\"" >> /etc/unbound/unbound.conf 
COPY unbound-pihole.conf /opt/unbound/

COPY entrypoint.sh entrypoint.sh

RUN chmod +x entrypoint.sh
ENTRYPOINT ./entrypoint.sh

#docker buildx build --platform linux/amd64 -t vertrou/pihole-unbound-test:latest --load