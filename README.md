Install Docker & Docker Compose on Ubuntu

Deploy Portainer

Deploy Nginx Proxy Manager (NPM)

Access both from outside via domain

Use NPM to reverse-proxy Portainer

Deploy Docker images using Portainer

============= After Successful Login to Web Server ==============

1. Install Docker and Docker Compose on Ubuntu

Use the command below:
```
sudo apt update
sudo apt install -y docker.io docker-compose
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
```

2. Set Up Project Directory

Command
```
mkdir ~/docker-stack && cd ~/docker-stack

```

3. Create docker-compose.yml

Paste this into the docker-compose.yml and save

```
version: '3'

services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: always
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data

  npm:
    image: jc21/nginx-proxy-manager:latest
    container_name: nginx-proxy-manager
    restart: always
    ports:
      - "80:80"
      - "81:81"
      - "443:443"
    environment:
      DB_SQLITE_FILE: "/data/database.sqlite"
    volumes:
      - npm_data:/data
      - npm_letsencrypt:/etc/letsencrypt

volumes:
  portainer_data:
  npm_data:
  npm_letsencrypt:
```

After save it run command

```
docker-compose up -d

```

4. Set Up Firewall

Command
```
sudo ufw allow OpenSSH
sudo ufw allow 80,443,9000/tcp
sudo ufw enable
```

5. Access the Apps

Portainer: http://your-server-ip:9000

Nginx Proxy Manager: http://your-server-ip:81
(Default login: admin@example.com / changeme)

============ For Domain ========
6. Point Your Domain (DNS)
Go to your DNS provider and create A records:
portainer.yourdomain.com -> your-server-ip


7. Set Up Nginx Proxy Manager for Portainer

Log into NPM (http://your-ip:81)

Add a new Proxy Host:

Domain Names: portainer.yourdomain.com

Forward Hostname/IP: portainer

Forward Port: 9000

Enable:

Websockets Support

Block Common Exploits

SSL tab:

Request a Let's Encrypt SSL cert

Enable HTTP/2 and HSTS


8. Access Portainer Using Domain
Visit: https://portainer.yourdomain.com
Use Portainer to manage and deploy Docker containers from here.

9. Deploy a Docker Container via Portainer
Log in to Portainer

Go to Containers > Add Container

Fill in:

Name: your-app

Image: nginx (or any valid Docker image)

Port Mapping: 80 → 8080 (example)

Click Deploy

Use NPM again to set up a reverse proxy for that container.