If above fails, install Docker Compose as standalone binary (official way):
Download latest Docker Compose binary:

bash
Copy
Edit
```DOCKER_COMPOSE_VERSION=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)
sudo curl -L "https://github.com/docker/compose/releases/download/$DOCKER_COMPOSE_VERSION/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose```
Make it executable:

bash
Copy
Edit
```sudo chmod +x /usr/local/bin/docker-compose```
Verify installation:

bash
Copy
Edit
```docker-compose --version```

https://chatgpt.com/s/t_6852a75511508191b0e2765766786358