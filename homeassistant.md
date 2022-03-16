# Home-assistant

sudo mkdir -p /homeassistant

```zsh
 docker run -d \
  --name homeassistant \
  --privileged \
  --restart=unless-stopped \
  -e TZ=MY_TIME_ZONE \
  -v /homeassistant:/config \
  --network=host \
  ghcr.io/home-assistant/home-assistant:stable
  ```