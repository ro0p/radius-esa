
# radius-esa
The goal of this service is to make 2FA available for any Radius capable devices in small environments without domain or any other LDAP based user management.

2FA is getting to be a must have requirement at least for VPN connections, but these environments typically use off-the-shelf routers with very limited VPN, and Radius capabilities, that is why this project only supports PAP authentication.

This project is NOT tested widely, use it on your own risk. Any comments, bug reports or suggestions are welcome.

## Features
- web management interface
- Radius server with handy client secret configuration
- local sqlite user database
- use ESET Secure Authentication via API
- windows service (sc) and foreground service mode

## Command line parameters
|  |  |
|--|--|
|-h, -help|display help|
|-c filename, -config filename|load configuration file|
|-d, -default|print default configuration|
|-s command, -service command|windows service commands:<br>*install*, *uninstall*, *start*, *stop*, *restart*, *status*|
|--cpuprofile filename|write cpu profile to file|
|--memprofile filename|write memory profile to file

## Configuration file
Defaults:
```json
{
  "database": "database.db",
  "api": {
    "listen": ":443",
    "network": "tcp4",
    "tls_mode": "auto"
  },
  "radius": {
    "address": ":1812",
    "network": "udp",
    "insecure": false,
    "static_secret": "",
    "client_secrets": null
  },
  "esa": {
    "address": "",
    "username": "",
    "password": "",
    "realm_id": ""
  },
  "log": {
    "debug_level": 0,
    "trace": false,
  }
}
```
### Radius
|  |  |
|--|--|
|address|server listen address in host:port form|
|insecure|if set to true Radius server will accept client connections without secret|
|static_secret|if not empty it will be used for all unknown clients|
|client_secrets|its a map with *client address* and *secret* pairs<br>client address can be in CIDR format, so subnets can be defined  easily|

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
|log|enable debug logging|

## Install & Usage on Windows
Copy the binary into a directory, install the service (Administrator privileges required):
```
radius-esa.exe -s install
```
You can find it in Windows Services as 'Radius ESA Wrapper'. Configure the service if you want and start it.
Configuration file is not neccessary in most cases, basic settings can be done via webui started on port 443 by default.

Using custom configuration file you can install the service like this:
```
radius-esa.exe -s install -c config.json
```
In service properties 'Path to executable' should contains this:
```
...\radius-esa.exe -s start -c config.json
```

The application generates a new self-signed certificate on every start. You can set a custom certificate and private key to prevent this behaviour.