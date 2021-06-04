# Pi-Hole + Updatelists Script + Unbound - 1 Container

## Description

This Docker deployment runs both Pi-Hole, Pihole updatelists script and Unbound in a single container. 

The base image for the container is the [official Pi-Hole container](https://hub.docker.com/r/pihole/pihole), with an extra build step added to install the Unbound resolver directly into to the container based on [instructions provided directly by the Pi-Hole team](https://docs.pi-hole.net/guides/unbound/). Additionally the [update script from jacklul](https://github.com/jacklul/pihole-updatelists) was added to the image

## Usage

First create a `.env` file to substitute variables for your deployment. Then create the folder on your host system for the volumes.
```
pihole-unbound/pihole/config/
pihole-unbound/pihole/dns/
pihole-unbound/pihole/pihole-updatelists/
pihole-unbound/unbound/
```
Copy your `unbound config` into the folder `~/pihole-unbound/unbound/`.
Get the [example config for the updatelists script](https://github.com/jacklul/pihole-updatelists/blob/master/pihole-updatelists.conf) and put it in the folder `pihole-unbound/pihole/pihole-updatelists/`.

> **_NOTE:_** It is also possible to start the container first with docker-compose. This automatically creates the folders and loads a few config files. After that, you can move your own config files into the folders. Then the container must be restarted once

---

### Required environment variables

> Vars and descriptions replicated from the [official pihole container](https://github.com/pi-hole/docker-pi-hole/):

| Docker Environment Var | Description|
| --- | --- |
| `ServerIP: <Host's IP>`<br/> | **--net=host mode requires** Set to your server's LAN IP, used by web block modes and lighttpd bind address
| `ServerIPv6: <Host's IPv6>`<br/> | Set to your server's LAN IPv6, used by web block modes and lighttpd bind address
| `TZ: <Timezone>`<br/> | Set your [timezone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) to make sure logs rotate at local midnight instead of at UTC midnight.
| `WEBPASSWORD: <Admin password>`<br/> | http://pi.hole/admin password. Run `docker logs pihole \| grep random` to find your random pass.
| `REV_SERVER: <"true"\|"false">`<br/> | Enable DNS conditional forwarding for device name resolution
| `REV_SERVER_DOMAIN: <Network Domain>`<br/> | If conditional forwarding is enabled, set the domain of the local network router
| `REV_SERVER_TARGET: <Router's IP>`<br/> | If conditional forwarding is enabled, set the IP of the local network router
| `REV_SERVER_CIDR: <Reverse DNS>`<br/>| If conditional forwarding is enabled, set the reverse DNS zone (e.g. `192.168.0.0/24`)

Example `.env` file in the same directory as your `docker-compose.yaml` file:

```
ServerIP=192.168.1.10
ServerIPv6=fd00::1:242:ac11:2 
TZ=America/Los_Angeles
WEBPASSWORD=QWERTY123456asdfASDF
REV_SERVER=true
REV_SERVER_DOMAIN=local
REV_SERVER_TARGET=192.168.1.1
REV_SERVER_CIDR=192.168.0.0/16
HOSTNAME=pihole
DOMAIN_NAME=pihole.local
```
### Running the stack

```bash
docker-compose up -d
```