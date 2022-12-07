# ZerotraefikOne

This stack uses ZerotierOne, the ztncui and traefik reverse proxy

ZeroTier combines the capabilities of VPN and SD-WAN, simplifying network management and emulates Layer 2 Ethernet with multipath, multicast, and bridging capabilities.
This softwarte is used to create zero-trust decentralized networks.
https://www.zerotier.com/

Traefik is used as a reverse proxy to be able to host additional webservices and for easier SSL certification.
https://traefik.io/
___
## Installation
The stack contains of the `ztncui` container, which includes the zerotier-one network controller and the web interface. In addition `traefik` is included as areverse proxy to handle SSL connection and certification as well as an access log.

### Create folders for persistant data
```
mkdir /zerotraefik  
mkdir /zerotraefik/traefik
```

### Get SSL certificate
#### Buy certificate
1. Create a certificat signing request:
```
openssl req -new -newkey rsa:2048 -nodes -keyout privkey.key -out server.csr
```
2. Buy certificat
3. Upload cert to `/zerotraefik/treafik`
4. Copy `privkey.key` into `/zerotraefik/treafik`
#### Certificate via Let's Encrypt
1. https://letsencrypt.org/getting-started/
2. Symlink certifiactes
```
ln -s /etc/letsencrypt/live/(your.domain.here)/cert.pem /zerotraefik/treafik/certificate.crt
```
```
ln -s /etc/letsencrypt/live/(your.domain.here)/privkey.pem /zerotraefik/treafik/privkey.key
```

### Traefik Configuration
Put `traefik.yaml` into `/zerotraefik/traefik` 

### Deploy via docker
Set password in `ztncui` under `environment` `- ZTNCUI_PASSWD=`
Set domain in `ztncui` under `environment` `- MYDOMAIN=`
```
docker-compose up -d
```
___
## Usage
Test deployment by goning to `https://(your.domain.here)` and login with the password set  in the `docker-compse` for `ztncui` under `environment` `- ZTNCUI_PASSWD`
### Create network
1. Log into the `ztncui` via browser in `https://zerotier.bnp.red`
2. Click `Add network`
3. Enter the `Network name` and click `Create Network`
4. Click on tab `Private` and ensure Access control is enabled
5. Click `Easy setup` and set network subnet and the start and end of the IP assignment pool
6. Finish by clicking the `Submit` Button
7. If a DNS server is implemented go to tab `DNS` and enter the `Domain` as well as the by zerotier asigned IP adresses for the DNS `Servers`

### Adding a client to the network
1. Download and install the zerotier-client for the operatingsystem which can be found at `https://www.zerotier.com/download/`
2. Join a network by entering the 16 digit `network id` shown for the network by the `ztncui`webinterface
3. Name and authorize the client in the webinterface
4. If needed set a static IP by clicking the assigned IP and set it and delete the automaticly assigned IP
