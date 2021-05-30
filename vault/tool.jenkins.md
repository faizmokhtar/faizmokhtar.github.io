---
id: 420ac6e2-a1db-40ea-8c0f-cfe93d8dca24
title: Jenkins
desc: ''
updated: 1621491761436
created: 1618755774345
---

## To install Jenkins with Docker

1. login to Docker
2. run the followings
````bash
$~ docker pull jenkins/jenkins:lts
$~ docker run --detach --publish 8080:8080 --volume
jenkins_home:/var/jenkins_home --name jenkins jenkins/jenkins:lts
$~ docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
````
3. copy the password and open localhost:8080
4. install suggested plugins

## To install Jenkins on Mac (Useful for iOS CI/CD)

1. Install it with `brew`
```bash
brew install --cask homebrew/cask-versions/adoptopenjdk8
brew install jenkins-lts
# run it in background
brew services start jenkins-lts
```

## Type of jobs in Jenkins

1. freestyle job (most commonly used)
2. pipeline job
3. multi-configuration job
4. github organization job
5. multibranch job