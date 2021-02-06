# Ez-Apps

## Overview
Docker-compose and unified config for EZ-App (**E**asy e**Z**lo) demonstration applications.  These applications illustrate how to use [Ezlo-Hub-Kit](https://github.com/bblacey/ezlo-hub-kit.git).

## Motivation

To simplify running and managing the suite of EZ-Apps using docker-compose.

#### EZ-Apps Managed by this Docker-compose
- [EZ-HouseMode-Synchronizer](https://github.com/bblacey/ez-housemode-synchronizer.git)
- [EZ-HouseMode-SceneRunnder](https://github.com/bblacey/ez-housemode-scenerunner.git)
- [EZ-MQTTRelay](https://github.com/bblacey/ez-mqttrelay.git)

## Installation & Usage
### 1. Either Download or Clone the EZ-Apps repository.
##### To download:
Click this [Repo Archive URL](https://github.com/bblacey/ez-apps/archive/main.zip) to download with your browser.  Or, if you prefer the command-line:
```
curl -LO https://github.com/bblacey/ez-apps/archive/main.zip
#  And unzip with your os-specific archive tool
```
##### To clone:
```
git clone https://github.com/bblacey/ez-apps.git
```

#### 2. Change your working directory to EZ-Apps
```
cd ez-apps
```

### 3. Configure the EZ-Apps
Edit [config.env](config.env) to use your MIOS username and password, Vera Serial # and MQTT broker URL.  If you want to customize the default EZ-HouseMode-SceneRunner scene-map, edit [scene-map.json](scene-map.json) accordingly.  Confirm and save your changes.

### 4. Start the EZ-Apps with `docker-compose`
```shell
$ docker-compose up -d
Pulling ez-mqttrelay (ghcr.io/bblacey/ez-mqttrelay:)...
latest: Pulling from bblacey/ez-mqttrelay
0a6724ff3fcd: Already exists
6dbd34771a81: Pull complete
6713a7e8bbfe: Pull complete
341b42aeff67: Pull complete
abd89b0e3dda: Pull complete
0cb8dafd95a4: Pull complete
Digest: sha256:a92ce4c17b14f4974302cbfaa645255def0c6d610dfdcc8c9f424b212ff47cf9
Status: Downloaded newer image for ghcr.io/bblacey/ez-mqttrelay:latest
Pulling ez-housemode-synchronizer (ghcr.io/bblacey/ez-housemode-synchronizer:)...
latest: Pulling from bblacey/ez-housemode-synchronizer
0a6724ff3fcd: Already exists
6dbd34771a81: Already exists
6713a7e8bbfe: Already exists
341b42aeff67: Already exists
ae12e8061264: Pull complete
1abff99f111c: Pull complete
Digest: sha256:42eea93938827e9c3ba1c065dabf9d9294383ac51f03f390cddb5035920a63c2
Status: Downloaded newer image for ghcr.io/bblacey/ez-housemode-synchronizer:latest
Pulling ez-housemode-scenerunner (ghcr.io/bblacey/ez-housemode-scenerunner:)...
latest: Pulling from bblacey/ez-housemode-scenerunner
0a6724ff3fcd: Already exists
6dbd34771a81: Already exists
6713a7e8bbfe: Already exists
341b42aeff67: Already exists
baa4493e0024: Pull complete
d0ce26223c48: Pull complete
Digest: sha256:029ee9adcb80b23ff89d673fddb4fad559085b9be3b6f1611f42c336c48b8d54
Status: Downloaded newer image for ghcr.io/bblacey/ez-housemode-scenerunner:latest
Creating ez-housemode-synchronizer ... done
Creating ez-mqttrelay              ... done
Creating ez-housemode-scenerunner  ... done
```
`docker-compose` will pull the latest docker image for each EZ-App for your architecture and the output should look similar the terminal session above.

### 5. Confirm they EZ-Apps started using `docker-compose logs`
Examine the docker-compose logs for a correct startup sequence.  It should look like the following with an ez-mqttrelay `Observering:`, ez-housemode-synchronizer `Synchronizing Housemode...` and ez-housemode-scenerunner `Managing HouseMode...` for each of your hubs.  For MQTT, ensure that the logs report that the ez-mqtt and the ez-housemode-syncrhonizer both connected to your broker and that the ez-housemode-synchronizer subscribed to the HouseModes1 topic for your Vera.

```shell
$ docker-compose logs
Attaching to ez-housemode-scenerunner, ez-mqttrelay, ez-housemode-synchronizer
ez-mqttrelay                 | Connected to mqtt broker mqtt://192.168.0.104
ez-mqttrelay                 | Observing: 92000014, architecture: armv7l	, model: h2_secure.1	, firmware: 2.0.7.1313.16, uptime: 0d 17h 57m 13s
ez-mqttrelay                 | Observing: 90000330, architecture: armv7l	, model: h2.1	, firmware: 2.0.7.1313.16, uptime: 0d 2h 22m 0s
ez-mqttrelay                 | Observing: 76002425, architecture: esp32	, model: ATOM32	, firmware: 0.8.528, uptime: 1d 16h 8m 33s
ez-housemode-synchronizer    | Connected to mqtt broker mqtt://192.168.0.104
ez-housemode-synchronizer    | Subscribed to HouseModes1 topic for Vera 50000796
ez-housemode-synchronizer    | Synchronizing HouseMode for: 76002425, architecture: esp32	, model: ATOM32	, firmware: 0.8.528, uptime: 1d 16h 8m 31s
ez-housemode-synchronizer    | Synchronizing HouseMode for: 90000330, architecture: armv7l	, model: h2.1	, firmware: 2.0.7.1313.16, uptime: 0d 2h 22m 23s
ez-housemode-synchronizer    | Synchronizing HouseMode for: 92000014, architecture: armv7l	, model: h2_secure.1	, firmware: 2.0.7.1313.16, uptime: 0d 17h 57m 36s
ez-housemode-scenerunner     | Using scene map
ez-housemode-scenerunner     | {
ez-housemode-scenerunner     |   "1": "Return",
ez-housemode-scenerunner     |   "2": "Leave",
ez-housemode-scenerunner     |   "3": "Sleep",
ez-housemode-scenerunner     |   "4": "Vacation"
ez-housemode-scenerunner     | }
ez-housemode-scenerunner     | Managing HouseMode Scenes for: 90000330, architecture: armv7l	, model: h2.1	, firmware: 2.0.7.1313.16, uptime: 0d 2h 21m 59s
ez-housemode-scenerunner     | Managing HouseMode Scenes for: 92000014, architecture: armv7l	, model: h2_secure.1	, firmware: 2.0.7.1313.16, uptime: 0d 17h 57m 13s
ez-housemode-scenerunner     | Managing HouseMode Scenes for: 76002425, architecture: esp32	, model: ATOM32	, firmware: 0.8.528, uptime: 1d 16h 8m 33s
```

Note, to monitor the logs continuously, you can use `docker-compose logs -f`
