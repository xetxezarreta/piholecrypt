# Piholecypt
Pi-hole + dnscrypt for Raspberry Pi.

<!--
<p align="center">
<a href="https://pi-hole.net"><img src="https://pi-hole.github.io/graphics/Vortex/Vortex_with_text.png" width="150" height="255" alt="Pi-hole"></a><br/>
</p>
-->


## Quick Start

1. Clone the repository
```bash
git clone https://github.com/xetxezarreta/piholecrypt.git
```
2. Update docker-compose.yml as needed. See example below:
```yaml
version: "3"

services:
  dnscrypt:
    container_name: dnscrypt
    build: ./dnscrypt
    networks:
      pihole_net:
        ipv4_address: 10.0.1.2
    expose:
      - "5300/udp"
      - "5300/tcp"
    dns:
      - 1.1.1.1
    volumes:
      - ./dnscrypt/config:/opt/dnscrypt-proxy/config
    restart: unless-stopped

  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    networks:
      pihole_net:
        ipv4_address: 10.0.1.3
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "80:80/tcp"
      #- "67:67/udp" # Only required if you are using Pi-hole as your DHCP server.
    environment:
      TZ: "Europe/Madrid" # Set the timezone.
      WEBPASSWORD: "admin" # Set a secure password here or it will be random.
      QUERY_LOGGING: "false" # Enable or disable query logging.
      DNS1: "10.0.1.2#5300" # Set pihole primary DNS server (dnscrypt-proxy).
      DNS2: "no" # Set pihole secondary DNS server.
    volumes:
       - ./pihole/etc-pihole/:/etc/pihole/
       - ./pihole/etc-dnsmasq.d/:/etc/dnsmasq.d/
    dns:
      - 1.1.1.1
    restart: unless-stopped
    depends_on:
      - dnscrypt

networks:
  pihole_net:
    driver: bridge
    ipam:
      config:
        - subnet: 10.0.1.0/24
```
3. Run `docker-compose up -d` to build and start pi-hole and dnscrypt containers.

## Upgrade running containers to latest version
1. Pull latest images.
```bash
docker-compose pull
```
2. Upgrade runnin containers using latest images. `up` command automatically recreates container on image or configuration change.
```bash
docker-compose up -d
```
