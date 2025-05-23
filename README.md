# GPTWOL a simple Wake/Sleep On Lan gui

---
[![Docker Pulls](https://img.shields.io/docker/pulls/misterbabou/gptwol.svg?logo=docker)](https://hub.docker.com/r/misterbabou/gptwol)
[![GitHub Release](https://img.shields.io/github/release/Misterbabou/gptwol.svg?logo=github&logoColor=959DA5)](https://github.com/Misterbabou/gptwol/releases/latest)
[![GitHub last commit](https://img.shields.io/github/last-commit/Misterbabou/gptwol?logo=github&logoColor=959DA5)](https://github.com/Misterbabou/gptwol/commits/main)
[![MIT Licensed](https://img.shields.io/github/license/Misterbabou/gptwol.svg?logo=github&logoColor=959DA5)](https://github.com/Misterbabou/gptwol/blob/main/LICENSE.md)
---

GPTWOL is a simple and lightweight Wake/Sleep on Lan gui made with python to wake up and shutdown your computers on your LAN.

## Screenshot 

| Light Web                         | Dark Web                           |
| --------------------------------- | ---------------------------------- |
| ![](/assets/gptwol-web-light.png) | ![](/assets/gptwol-web-dark.png)   |

| Light Mobile                      | Dark Mobile                        |
| --------------------------------- | ---------------------------------- |
| ![](/assets/gptwol-mob-light.png) | ![](/assets/gptwol-mob-dark.png)   |

## Features 

- Docker Image to deploy
- Send Wake On Lan packets to wake up computers
- Send Sleep On Lan packets to shutdown computers
- Add or Delete Computer
- Computers status check with ping or tcp request (timeout settings available)
- Very low power usage (20 mb RAM)
- Check if IP and MAC provided are valid
- cron job to wake up device
- Check if Cron provided is valid
- Search on computer Name, MAC or IP
- Dark mode
- Authentication (disable by default)

## Special configuration you can change

- Ping Refresh to check Status availability 
- Disable Delete or Add Computers
- Change the port of the Web UI
- Enable authentication

![](/assets/authentication.png)

## Docker Configuration
> [!NOTE]
>
>It's recommended to use docker compose to run this application. [Install documentation](https://docs.docker.com/compose/install/)

> [!CAUTION]
>
>- The app container needs to run in host network mode to send the wakeonlan command on your local network.
>- Make sure that the PORT you are using is free on your host computer
>- Make sure that BIOS settings and remote OS is configure to allow Wake On Lan
>- Don't expose gptwol directly on internet without proper authentication

### With docker compose

Create `docker-compose.yml` file:
```
services:
  gptwol:
    container_name: gptwol
    image: misterbabou/gptwol:latest
    network_mode: host
    restart: unless-stopped
    environment:
      - PORT=8080 #Free Port on Your host; default is 5000
      - TZ=Europe/Paris #Set your timezone for Cron; default is UTC
      #- ENABLE_LOGIN=true #Uncomment this line to enable basic login; You would be able to access with USERNAME and PASSWORD; default is false
      #- USERNAME=admin #Uncomment this line to set a username; default is admin
      #- PASSWORD=admin #Uncomment this line to set a password; default is admin
      #- SCRIPT_NAME=/my-app #Uncomment this line to run the app under a prefix
      #- DISABLE_ADD_DEL=1 #Uncomment this line to disable Add or delete Computers; default is to allow
      #- DISABLE_REFRESH=1 #Uncomment this line to prevent your browser to refresh Computer status; default is to allow
      #- REFRESH_INTERVAL=15 # Uncomment this line to change status check for icmp ou tcp, can be 15 or 60 (seconds); default value is 30 seconds
      #- PING_TIMEOUT=200 #Uncomment this line to change the time to wait for a ping answer in (in ms); default value is 300 milliseconds
      #- TCP_TIMEOUT=5 #Uncomment this line to change the time to wait for a tcp check (in s);  default value 1 second
    volumes:
      - ./appdata/db:/app/db
      - ./appdata/cron:/etc/cron.d
```

Run the application
```
docker compose up -d
```

### With docker

Run the application
```
docker run -d \
  --name=gptwol \
  --network="host" \
  --restart unless-stopped \
  -e PORT=8080 \
  -e TZ=Europe/Paris \
  -v ./appdata/db:/app/db \
  -v ./appdata/cron:/etc/cron.d \
  misterbabou/gptwol:latest
```

## Configure Sleep on Lan

- Check the [Sleep on Lan Github](https://github.com/SR-G/sleep-on-lan) repo to download and configure
- GPTWOL send a reverse MAC wakeonlan packet on port 9 to shutdown your computer (you don't need to configure API)

Here is an example of a wol.json to shutdown a Debian based computer
```
{
    "Listeners": [
        "UDP:9"
    ],
    "LogLevel": "INFO",
    "Commands": [
        {
            "Operation": "shutdown",
            "Command": "poweroff",
            "Default": true
        }
    ]
}
```

## Roadmap 

:heavy_check_mark: Add ARM version (Added in 1.0.1)

:heavy_check_mark: Add feature to plan automatic Wake on Lan (Cron) (Added in 1.0.3)

:heavy_check_mark: Add Search feature (Added in 1.0.4)

:heavy_check_mark: Remove Cron on Computer deletion (Added in 1.0.4)

:heavy_check_mark: Improve load page performance due to ping timeout. (added in 1.0.5)

:heavy_check_mark: Add a TCP port option to check availability without using ICMP (added in 2.0.1)

:heavy_check_mark: Run app on subpath (added in 2.1.0)

:heavy_check_mark: Make app responsive for smaller screen (added in 2.1.0)

:heavy_check_mark: Add Dark Mode Switch (added in 2.1.3)

:heavy_check_mark: move computers.txt in an other directory not to mount a file but a directory to the docker container (added in 4.0.0)

:heavy_check_mark: Shutdown computers with Sleep on LAN (added in 4.1.0)

:heavy_check_mark: Add optional authentication (added in 4.2.0)

- Add filter buttons to filter computer by Name or IP

- Add OIDC Authentication (will require time)
