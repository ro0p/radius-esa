
# radius-esa
The goal of this service is to make 2FA available for any Radius capable devices in small environments without domain or any other LDAP based user management.
These environments typically use off-the-shelf routers with lightweight VPN support and have only desktop windows computers, but 2FA is getting to be a must have requirement at least for VPN connections.

This project is NOT tested widely, use it on your own risk. Any comments, bug reports or suggestions are welcome.

## Features
- Radius server with handy client secret configuration
- local sqlite user database with Bcrypt passwords
- use ESET Secure Authentication via API
- windows service (sc) and foreground service mode

## Command line parameters
|  |  |
|--|--|
|-h, --help|display help|
|-c filename, --config filename|load configuration file (default: config.json)|
|-s command, --service command|windows service commands:<br>*install*, *uninstall*, *start*, *stop*, *restart*, *status*|
|-a username, --add username|add user to database, password will be asked|
|--cpuprofile filename|write cpu profile to file|
|--memprofile filename|write memory profile to file

## Configuration file
Defaults:
```json
{
  "radius": {
    "address": ":1812",
    "insecure": false,
    "static_secret": "",
    "secrets": {}
  },
  "esa": {
    "address": "",
    "username": "",
    "password": "",
    "realm_id": ""
  },
  "database": "database.db",
  "debug": false
}
```
### Radius
|  |  |
|--|--|
|address|server address in host:port form|
|insecure|if set to true Radius server will accept client connections without secret|
|static_secret|if not empty it will be used for all unknown clients|
|secrets|its a map with *client address* and *secret* pairs<br>client address can be in CIDR format, so subnets can be defined  easily|

### ESA
|  |  |
|--|--|
|address|ESA API endpoint URL, eg: https://example.com:8001<br>can be a comma separated list in HA environments|
|username|ESA 'API key' account|
|password|ESA 'API key' password|
|realm_id|the user's realm. if empty, use default realm in ESA|

### Other settings
|  |  |
|--|--|
|database|filename of the sqlite database|
|debug|enable debug logging|

