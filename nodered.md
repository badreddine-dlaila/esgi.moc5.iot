# Nodered
```zsh
bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)

sudo apt-get update --allow-releaseinfo-change -v
sudo apt-get upgrade
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -

docker run -it --restart=unless-stopped -p 1880:1880 --name nodered nodered/node-red
```