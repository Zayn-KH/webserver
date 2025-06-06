# Recreating Docker Image
For everytimes recreating image don't forget to take a look of any line

This is the original 
```
docker run -d \
  --name jenkins \
  --user root \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /usr/bin/docker:/usr/bin/docker \
  jenkins/jenkins:lts
```

This is the new update
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

At the moment we want to update only time zone but actually we have remove docker volume
```
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /usr/bin/docker:/usr/bin/docker \
```

So after recreating image it will cause any error in Jenkins.
Here is the correct update
```
docker run -d \
  --name jenkins \
  --user root \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /usr/bin/docker:/usr/bin/docker \
  -e TZ=Asia/Phnom_Penh \
  -v /etc/timezone:/etc/timezone:ro \
  -v /etc/localtime:/etc/localtime:ro \
  jenkins/jenkins:lts
```