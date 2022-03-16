# Homebridge stack
References :
* https://github.com/homebridge/homebridge/wiki/Install-Homebridge-on-Docker

```yaml 
# Cearte a new stack named : homebridge 
version: '2'
services:

  homebridge:
    image: oznu/homebridge
    container_name: homebridge
    restart: unless-stopped
    network_mode: host
    ports:
      - 8181:8080
    volumes:
      - /srv/HomeBridge:/homebridge
    environment:
     # In this instance PUID=1001 and PGID=1001. To find yours use id user as below:
     # id <dockeruser>
      - PUID=1000                  
      - PGID=1000
      - HOMEBRIDGE_CONFIG_UI=1
      - HOMEBRIDGE_CONFIG_UI_PORT=8181
```

```js
// Bulb MqttThings config sample 
"topics": {
    "getOn": {
        "topic": "zigbee2mqtt/0x00178801085aff46",
        "apply": "return JSON.parse(message).state"
    },
    "setOn": "zigbee2mqtt/0x00178801085aff46/set",
    "setHSV": {
        "topic": "zigbee2mqtt/0x00178801085aff46/set",
        "apply": "return JSON.stringify({transition: 2, brightness: Math.round(message.substr(message.lastIndexOf(',')+1,message.length)* 2.55), color: {hue: message.substr(0,message.indexOf(',')), saturation: message.substr(message.indexOf(',')+1,(message.lastIndexOf(',')-message.indexOf(','))-1)}})"
    }
}
```