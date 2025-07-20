# Homelab

services:
############################
# QBITTORRENT
############################

  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    ports:
      - 8080:8080 #qbittorrent
      - 6881:6881 #qbittorrent
      - 6881:6881/udp #qbitorrent
    network_mode: "arrstack_default"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
      - WEBUI_PORT=8080
      - TORRENTING_PORT=31890
    volumes:
      - /media/ArrStack/qbittorrent/config:/config
      - /media/ArrFiles/Downloads:/downloads #optional
    restart: never

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
      - /media/ArrStack/jellyfin/config:/config
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
    network_mode: "arrstack_default"
    ports: 
      - 9696:9696
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
    volumes:
      - /media/ArrStack:/config
    restart: unless-stopped

############################
# PLEX
############################

  plex:
    image: lscr.io/linuxserver/plex:latest
    container_name: plex
    network_mode: "arrstack_default"
    ports:
      - 32400:32400
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - VERSION=docker
      - PLEX_CLAIM= #optional
    volumes:
      - /media/ArrStack/plex/library:/config
      - /media/ArrFiles/TV_Shows:/TV_Shows
      - /media/ArrFiles/Movies:/Movies
    restart: unless-stopped

############################
# RADARR
############################

  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    network_mode: "arrstack_default"
    ports:
      - 7878:7878
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
    volumes:
      - /media/ArrStack/radarr/data:/config
      - /media/ArrFiles/Movies:/movies #optional
      - /media/ArrFiles/Downloads:/downloads #optional
    restart: unless-stopped

############################
# OVERSEERR
############################

  overseerr:
    image: lscr.io/linuxserver/overseerr:latest
    container_name: overseerr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
    volumes:
      - /media/ArrStack/overseerr/config:/config
    ports:
      - 5055:5055
    restart: unless-stopped
