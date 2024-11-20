Docker Lab 3

# Wireguard Docker Container Setup Guide

---

### Prerequisites
- Create a DigitalOcean account.
- Use this link to [DigitalOcean.com](https://www.digitalocean.com/) to sign up with your school email and Agent Miller’s credit card.

---

## Create a Droplet in DigitalOcean

### Droplet Settings:
- Choose the lowest price tier: `$6 per month`.
- Select an **Ubuntu droplet** with all basic options.
- Use a password instead of an SSH key:
  - Password: `TulsaTime@1865edu`
- Name the project: `Wireguard Project`
- Droplet IP: `64.23.171.188`

---

## Install Wireguard

### SSH into the droplet:
`ssh root@64.23.171.188`

---

### Install Docker, Docker-Compose, and Wireguard:
`sudo apt update`  
`sudo apt install docker.io`  
`sudo apt install docker-compose`  
`sudo apt install wireguard`

---

## Set Up Wireguard Using Docker Compose

### Create a directory for Wireguard:
`mkdir -p /opt/wireguard`  
`cd /opt/wireguard`

---

### Create the `docker-compose.yml` file:
`nano docker-compose.yml`

---

### Add the following configuration to `docker-compose.yml`:
```yaml
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=American/New_York
      - SERVERURL=45.55.41.235
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
