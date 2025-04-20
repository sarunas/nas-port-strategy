# NAS Port Strategy

This document outlines a port allocation strategy for managing Docker containers and services on a Synology NAS device. The strategy aims to avoid port conflicts with Synology DSM and built-in services while ensuring a clean, scalable networking configuration for all containerized applications.

## Overview

The goal is to:
- Avoid port conflicts with Synology DSM and built-in services.
- Organize ports by application category.
- Enable clean and scalable service management with reverse proxies.

## Key Concepts

### Reverse Proxy (Host Mode)

- Runs in `network_mode: host`.
- Listens on host ports 80 (HTTP) and 443 (HTTPS).
- Routes traffic to backend services using internal ports mapped to unique host ports.

### Synology Reserved Ports to Avoid

| Port        | Service                  |
| ----------- | ------------------------ |
| 5000/5001   | DSM Web UI (HTTP/HTTPS)  |
| 32400       | Plex                     |
| 9001        | Surveillance Station     |
| 5006        | Synology Chat            |
| 6690        | Synology Drive           |
| 5310–5320   | Docker GUI backend       |
| 17000–18000 | DLNA/Audio Station, etc. |
| 10101–10110 | Synology Active Insight  |

## Port Range Allocation Strategy

| Port Range | Category             | Example Apps                             |
| ---------- | -------------------- | ---------------------------------------- |
| 6000–6099  | Media Management     | Sonarr, Radarr, Lidarr, Prowlarr, Bazarr |
| 6100–6199  | Media Playback       | Jellyfin, Emby                           |
| 6200–6299  | Photo Management     | PhotoPrism, Immich                       |
| 6300–6399  | Smart Home           | Home Assistant, ESPHome, Zigbee2MQTT     |
| 6400–6499  | Video Surveillance   | Frigate, Shinobi, MotionEye              |
| 6500–6599  | Personal Cloud       | Nextcloud, FileBrowser, Syncthing        |
| 6600–6699  | Network Management   | GenieACS, Pi-hole, AdGuard Home          |
| 6700–6799  | Dashboards/Dev Tools | Portainer, Netdata, Grafana, Dashy       |
| 6800–6899  | Databases            | PostgreSQL, InfluxDB, MariaDB            |
| 6900–6999  | Miscellaneous        | Other apps not in above categories       |

## Sample Port Assignments

### Media Management (6000–6099)

| App      | Host Port | Container Port |
| -------- | --------- | -------------- |
| Sonarr   | 6001      | 8989           |
| Radarr   | 6002      | 7878           |
| Lidarr   | 6003      | 8686           |
| Prowlarr | 6004      | 9696           |
| Bazarr   | 6005      | 6767           |

### Smart Home (6300–6399)

| App            | Host Port | Container Port |
| -------------- | --------- | -------------- |
| Home Assistant | 6301      | 8123           |
| ESPHome        | 6302      | 6052           |

### Surveillance (6400–6499)

| App     | Host Port | Container Port |
| ------- | --------- | -------------- |
| Frigate | 6401      | 5000           |

### Personal Cloud (6500–6599)

| App       | Host Port | Container Port |
| --------- | --------- | -------------- |
| Nextcloud | 6501      | 80             |
| Syncthing | 6502      | 8384           |

### Network Management (6600–6699)

| App      | Host Port | Container Port |
| -------- | --------- | -------------- |
| GenieACS | 6601      | 7547           |
| Pi-hole  | 6602      | 80             |
