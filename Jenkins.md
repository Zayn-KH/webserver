# To get jenkins password for the first initial

```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Note: Must change the jenkins_home to your actual jenkins volume like below is jenkins_data
```
    volumes:
      - jenkins_data:/var/jenkins_data
```

# To run inside docker jenkins
```
docker exec -u root -it jenkins bash
```