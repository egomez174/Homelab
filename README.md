# Homelab

services:
############################
# QBITTORRENT
############################

  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
      - WEBUI_PORT=8080
      - TORRENTING_PORT=6881
    volumes:
      - /media/arr/qbittorrent/config:/config
      - /media/arr/qbittorrent/downloads:/downloads #optional
    restart: unless-stopped

############################
# JELLYFIN
############################

  jellyfin:
    image: lscr.io/linuxserver/jellyfin:latest
    container_name: jellyfin
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
    volumes:
      - /media/ArrFiles/jellyfin/config:/config
      - /media/ArrFiles/Movies:/Movies
      - /media/ArrFiles/TV_Shows:/TV_Shows
    ports:
      - 8096:8096
      - 8920:8920 #optional
      - 7359:7359/udp #optional
      - 1900:1900/udp #optional
    restart: unless-stopped

############################
# GLUETUN
############################

  gluetun:
    image: qmcgaw/gluetun
    container_name: gluetun
    ports:
      - 8080:8080 #qbittorrent
      - 6881:6881 #qbittorrent
      - 6881:6881/udp #qbitorrent
      - 9696:9696 #prowlarr
    cap_add:
      - NET_ADMIN
    devices:
      - /dev/net/tun:/dev/net/tun
    environment:
      - VPN_SERVICE_PROVIDER=ipvanish
      - OPENVPN_USER=XXXXXX@gmail.com
      - OPENVPN_PASSWORD=XXXXXXXXX
      - SERVER_COUNTRIES=United States

############################
# PROWLARR
############################

  prowlarr:
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
    volumes:
      - /media/ArrStack:/config
    restart: unl
