FROM pihole/pihole:v5.8.1

RUN apt-get update && apt-get install -Vy php-cli php-sqlite3 php-intl php-curl

RUN wget -O - https://raw.githubusercontent.com/jacklul/pihole-updatelists/master/install.sh | sudo bash /dev/stdin docker

RUN apt-get install -y unbound

RUN echo "include: \"/opt/unbound/*.conf\"" >> /etc/unbound/unbound.conf 

COPY entrypoint.sh entrypoint.sh

RUN chmod +x entrypoint.sh
ENTRYPOINT ./entrypoint.sh