# Parameters

## Volumes

| Parameter | Function |
| --- | --- |
| `/path/to/7DaysToDie:/home/sdtdserver/.local/share/7DaysToDie/` | 7DaysToDie saves, where maps are store. |
| `/path/to/ServerFiles:/home/sdtdserver/serverfiles/` | 7DaysToDie server config files. |
| `/path/to/Logs:/home/sdtdserver/log/` | 7DaysToDie server log files. |
| `/path/to/BackupFolder:/home/sdtdserver/lgsm/backup/` | 7DaysToDie server backups files. |
| `/path/to/LGSM-Config:/home/sdtdserver/lgsm/config-lgsm/sdtdserver/` | LGSM config files. [More info](https://docs.linuxgsm.com/commands/monitor) |

## Ports

| Parameter | Function |
| --- | --- |
| `26900:26900/tcp` | Default 7DaysToDie port **required** |
| `26900:26900/udp` | Default 7DaysToDie port **required** |
| `26901:26901/udp` | Default 7DaysToDie port **required** |
| `26902:26902/udp` | Default 7DaysToDie port **required** |
| `8080:8080/tcp` | Default 7DaysToDie webadmin port **optional**, if you use webadmin remember to change password in `/path/to/ServerFiles/sdtdserver.xml` |
| `8081:8081/tcp` | Default 7DaysToDie telnet port **optional** |
| `8082:8082/tcp` | Default [Alloc Fixes Map GUI](https://7dtd.illy.bz/wiki/Server%20fixes) webserver port **optional** |

### General Tips about Port Mapping the Game Port 

If you set the Game Port to 26900 (default port) the server will automatically set two consecutive ports: 26901 and 26902.

If you want to change de default Game Port (26900) only port mapping wont work, because steam will announce the server in the default port (26900) and players will be unable to join. Necessary to edit the /ServerFiles/sdtdserver.xml and update the game port there too.

In case you have 2 containers in the Same Server, you can use the default ports in Container 1 and update the Container 2 to something like:

| Container 1 | Container 2 |
| --- | --- |
| `26900:26900/tcp` | `26905:26905/tcp` # Update /ServerFiles/sdtdserver.xml: property name="ServerPort" value="26905" |
| `26900:26900/udp` | `26905:26905/udp` # Update /ServerFiles/sdtdserver.xml: property name="ServerPort" value="26905" |
| `26901:26901/udp` | `26906:26906/udp` # Automatically and consecutive allocated after the defined ServerPort choosen |
| `26902:26902/udp` | `26907:26907/udp` # Automatically and consecutive allocated after the defined ServerPort choosen |
| `8080:8080/tcp` | `8090:8080/udp` #Can be mapped without consequences |
| `8081:8081/tcp` | `8091:8081/udp` #Can be mapped without consequences |
| `8082:8082/tcp` | `8092:8082/udp` #Can be mapped without consequences |


### Troubleshooting Connection Issues
| Error | Fix |
| --- | --- |
| Steam Announces the wrong Game Port | Check /ServerFiles/sdtdserver.xml |
| NET: LiteNetLib: Connection failed: ConnectionFailed | The two consecutive ports automatically defined after the game port are closed on the server or incorrectly mapped. |

## General

| Parameter | Function | Values |
| --- | --- | :---: |
| `START_MODE=1` | Start mode of the container **required** | [Start Modes](parameters.md#start-modes) |
| `TimeZone=Europe/Madrid` | for TimeZone **recomendable**| [TZ Database](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) |
| `PUID=1000` | for UserID | [More info](user_groups_identifiers.md) |
| `PGID=1000` | for GroupID | [More info](user_groups_identifiers.md) |
| `--restart unless-stopped` | Restart container always unlesss stopped manually **NEVER USE WITH START_MODE=0, 2 or 4** | [Restart Policy](https://docs.docker.com/config/containers/start-containers-automatically/#use-a-restart-policy) |
| `CHANGE_CONFIG_DIR_OWNERSHIP` | Disable chown of config dirs | [More info](parameters.md#directory-ownership) 

## 7 Days to Die

| Parameter | Function | Values |
| --- | --- | :---: |
| `VERSION=stable` | Change between 7 days to die versions/branches **optional** | [Branches](https://steamdb.info/app/251570/depots/) |

## LinuxGSM

| Parameter | Function | Values |
| --- | --- | :---: |
| `BACKUP=NO` | Backup server at 5 AM (Only the latest 5 backups will be keep, maximum 30 days) [More info](https://docs.linuxgsm.com/commands/backup) **optional** | YES, NO |
| `MONITOR=NO` | Monitor server status, if server crash this will restart it [More info](https://docs.linuxgsm.com/commands/monitor) **optional** | YES, NO |
| `TEST_ALERT=NO` | Test alerts at start of server **optional** | YES, NO |

## Mods

### Global

| Parameter | Function | Values |
| :---: | --- | :---: |
| `UPDATE_MODS=NO` | Allow mods to be update before server start must be used with each mod XXX_UPDATE=YES **optional** | YES, NO |

### Provided user mods

| Parameter | Function | Values |
| :---: | --- | :---: |
| `MODS_URLS=""` | Allow mods to be installed from URL, support zip and rar **optional** | "URL1,URL2" |

### Overhaul mods

#### Undead Legacy

| Parameter | Function | Values |
| --- | --- | :---: |
| `UNDEAD_LEGACY=NO` | Install [Undead Legacy](https://ul.subquake.com/), if DARKNESS_FALLS it's enable too will not be installed  **optional** | YES, NO |
| `UNDEAD_LEGACY_VERSION=stable` | Install [Undead Legacy Versions](https://ul.subquake.com), [CHECK BUILDS COMPATIBILITY](https://ul.subquake.com/download) **optional** | EXP, STABLE |
| `UNDEAD_LEGACY_UPDATE=NO` | Update [Undead Legacy](https://ul.subquake.com/patch-notes), on server install, requiere UPDATE_MODS=YES, if you enable this environment, the mod will be update every time you start the server and will take sometime. **optional** | YES, NO |
| `UNDEAD_LEGACY_URL=False` | Install a custom [Undead Legacy](https://ul.subquake.com/) **optional** | Must be a valid url, this method don't add the custom XML settings of UL |

#### Darkness Falls

| Parameter | Function | Values |
| --- | --- | :---: |
| `DARKNESS_FALLS=NO` | Install [Darkness Falls](https://community.7daystodie.com/topic/4941-darkness-falls-they-mostly-come-out-at-night/), if UNDEAD_LEGACY it's enable too will not be installed **optional** | YES, NO |
| `DARKNESS_FALLS_UPDATE=NO` | Update [Darkness Falls](https://community.7daystodie.com/topic/4941-darkness-falls-they-mostly-come-out-at-night/), on server install, requiere UPDATE_MODS=YES, if you enable this environment, the mod will be update every time you start the server and will take sometime. **optional** | YES, NO |
| `DARKNESS_FALLS_URL=False` | Install a custom [Darkness Falls Version](https://community.7daystodie.com/topic/4941-darkness-falls-they-mostly-come-out-at-night/), [CHECK BUILDS COMPATIBILITY](https://community.7daystodie.com/topic/4941-darkness-falls-they-mostly-come-out-at-night/) **optional** | Must be a git url |

### Alloc Fixes

| Parameter | Function | Values |
| :---: | --- | :---: |
| `ALLOC_FIXES=NO` | Install [Allocs Fixes](https://7dtd.illy.bz/wiki/Server%20fixes), Alloc Fixes version will be installed according to 7 days to die branch version  **optional** | YES, NO |
| `ALLOC_FIXES_UPDATE=NO` | Update [Allocs Fixes](https://7dtd.illy.bz/wiki/Server%20fixes), depending of your 7 days to die version, on server install, requiere UPDATE_MODS=YES **optional** | YES, NO |

### CSMM Patron's Mod (CPM)

| Parameter | Function | Values |
| --- | --- | :---: |
| `CPM=NO` | Install [CPM](https://docs.csmm.app/en/cpm/), CPM version will be installed according to 7 days to die branch version, this will also install Allocs Fixes **optional** | YES, NO |
| `CPM_UPDATE=NO` | Update [CPM](https://docs.csmm.app/en/cpm/), CPM on every server start, depending of your 7 days to die version, requiere UPDATE_MODS=YES **optional** | YES, NO |

### BepInEx

| Parameter | Function | Values |
| --- | --- | :---: |
| `BEPINEX=NO` | Install [BEPINEX](https://github.com/BepInEx/BepInEx), latest BEPINEX version will be installed**optional** | YES, NO |
| `BEPINEX_UPDATE=NO` | Update [BEPINEX](https://github.com/BepInEx/BepInEx), BEPINEX on every server start, requiere UPDATE_MODS=YES **optional** | YES, NO |

### START MODES

| START_MODE | Information |
| :----: | ---- |
| 0 | Install server and STOP the container |
| 1 | Start server |
| 2 | Update server and STOP the container |
| 3 | Update and start start |
| 4 | Backup server and STOP the container |


### DIRECTORY OWNERSHIP ### 
Sometimes if you're running some of your volumes on a NAS, you wont be the owner of the volumes you mount, and wont have access to change their ownership. This setting will bypass trying to change ownership, otherwise the startup will fail. 

#### WARNING

IF YOU UPDATE FROM STABLE TO EXPERIMENTAL OR VICE VERSA, REMEMBER TO BACKUP FIRST YOUR SERVER TO AVOID ANY ERRORS.
