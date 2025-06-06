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
    environment:
      - TZ=Asia/Phnom_Penh
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - portainer_data:/data
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro

  npm:
    image: jc21/nginx-proxy-manager:latest
    container_name: nginx-proxy-manager
    restart: always
    ports:
      - "80:80"
      - "81:81"
      - "443:443"
    environment:
      - DB_SQLITE_FILE=/data/database.sqlite
      - TZ=Asia/Phnom_Penh
    volumes:
      - npm_data:/data
      - npm_letsencrypt:/etc/letsencrypt
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro

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




====================== Jenkins Docker Create =======================
run command 
```
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -e TZ=Asia/Phnom_Penh \
  -v /etc/timezone:/etc/timezone:ro \
  -v /etc/localtime:/etc/localtime:ro \
  jenkins/jenkins:lts

```

Or with docker-compose.yml
```
services:
  jenkins:
    image: jenkins/jenkins:lts
    ports:
      - "8080:8080"
      - "50000:50000"
    environment:
      - TZ=Asia/Phnom_Penh
    volumes:
      - jenkins_home:/var/jenkins_home
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
```
And then run ``` docker-compose up -d ``` to create image.



=============== TIPS OF RECREATE OR UPDATE DOCKER CONTAINER ==============
Do not run ``` docker-compose down ``` 
…it tried to remove everything the Compose file created, including:

Containers ✅

Volumes ✅ (if -v is added)

The default network ❌ ← this is what caused the error because other containers are using it.


Use ``` docker-compose stop ``` and ``` docker-compose up -d ``` instead:

# Stop containers only (but do NOT remove network)
docker-compose stop

# Recreate containers with updated settings (e.g. new TZ)
docker-compose up -d
