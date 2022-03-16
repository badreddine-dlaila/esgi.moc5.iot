# Mossquitto 
* First we create the following folders under a mosquitto folder in your default directory: config, data and log
```zsh
sudo mkdir -p /mosquitto/config
sudo mkdir -p /mosquitto/data
sudo mkdir -p /mosquitto/log
.
│ 
└───config
│   └─── mosquitto.conf
└───data
│   └─── log.db
└───log
    └─── mosquitto.log
```

```zsh
# In the config folder we create a file called mosquitto.conf with the following command
sudo nano /mosquitto/config/mosquitto.conf
# Now we have to make sure that Mosquitto as a Docker container get the permission to access these folders
sudo chown -R 1883:1883 /mosquitto

# Paste this base mosquitto config in mosquitto.conf : https://github.com/eclipse/mosquitto/blob/master/mosquitto.conf

# Create and start the Mosquitto container with the following command
sudo docker run --name=mosquitto --restart=unless-stopped --net=host -tid -p 1883:1883 -p 9001:9001 -v /mosquitto/config:/mosquitto/config:ro -v /mosquitto/log:/mosquitto/log -v /mosquitto/data/:/mosquitto/data/ -itd  eclipse-mosquitto

# Note the output & see https://mosquitto.org/documentation/migrating-to-2-0/:
Starting in local only mode. Connections will only be possible from clients running on this machine.
1613430248: Create a configuration file which defines a listener to allow remote access.
1613430248: Opening ipv4 listen socket on port 1883.
1613430248: Opening ipv6 listen socket on port 1883.

# Enable connection from a remote machine by adding a listener
listener 1883

# Log into your container console via portainer
# Create passwd file 
mosquitto_passwd -c /mosquitto/config/mosquitto.passwd <USER>
# Edit mosquitto.conf & add :
password_file /mosquitto/config/mosquitto.passwd
allow_anonymous false
```

# Zigbee2Mqtt

References :
* https://www.zigbee2mqtt.io/
* https://www.zigbee2mqtt.io/getting_started/running_zigbee2mqtt.html
* https://www.zigbee2mqtt.io/guide/adapters/flashing/flashing_the_cc2531.html
* https://www.zigbee2mqtt.io/guide/configuration/zigbee-network.html#network-config

* First we create the following folders under a mosquitto folder in your default directory: config, data and log
```zsh
sudo mkdir -p /zigbee2mqtt/
sudo mkdir -p /zigbee2mqtt/data
.
│ 
└───zigbee2mqtt
│   └──data
```

```zsh
sudo docker run -itd --name="zigbee2mqtt" --net=host -e TZ=Europe/Paris --device=/dev/ttyACM0 --restart=unless-stopped -v /zigbee2mqtt/data:/app/data koenkk/zigbee2mqtt:latest
```