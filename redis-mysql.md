# Check if we can boot required redis and mysql container from gitlab pipeline itself.

In order to Run redis and mysql container in git lab pipeline , we need to follow these step 

# GitLab Runner with the Docker executor

 Make sure docker in installed in host machine 
## for ubuntu 
```
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
```
for ubuntu 
now run the installation command 

```
sudo apt-get install gitlab-runner
```
## for mac based on intel download the registry 

```
sudo curl --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-darwin-amd64"
```

give permisiion to execute 

```
sudo chmod +x /usr/local/bin/gitlab-runner
```
install 

```
cd ~
gitlab-runner install
gitlab-runner start

```

for login inside gitlab 

```
sudo gitlab-runner register
```

# Configure Docker Executor 

open configuration files 

on macos

```

open ~/Library/Application\ Support/gitlab-runner/config.toml

```

confirm or add [[runners]] , YOUR_REGISTRATION_TOKEN and required image 

```
[[runners]]
  name = "My Docker Runner"
  url = "https://gitlab.com/"
  token = "YOUR_REGISTRATION_TOKEN"
  executor = "docker"
  [runners.docker]
    tls_verify = false
    image = "docker:stable"
    privileged = true
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache"]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]


```

# set up the .gitlab-ci.yml 

set up .gitlab-ci.yml or reconfigure existing for declaring mysql and redis 

```
image: docker:stable

stages:
  - test

services:
  - docker:dind

variables:
  MYSQL_ROOT_PASSWORD: mysecretpassword

test:
  stage: test
  script:
    - docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=$MYSQL_ROOT_PASSWORD mysql:latest
    - # Set up your application and run tests
    - # Clean up the MySQL container
    - docker stop mysql-container
    - docker rm mysql-container

```

## you need to modified as per project's requirement .

you can also add other variables such as 

```

image: docker:stable

services:
  - name: mysql:latest
    alias: mysql
  - name: redis:latest
    alias: redis

stages:
  - test

variables:
  MYSQL_ROOT_PASSWORD: mysecretpassword
  MYSQL_DATABASE: mydatabase
  REDIS_PASSWORD: myredispassword

test:
  stage: test
  script:
    - docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=$MYSQL_ROOT_PASSWORD -e MYSQL_DATABASE=$MYSQL_DATABASE --network=service_mysql mysql:latest
    - docker run -d --name redis-container -e REDIS_PASSWORD=$REDIS_PASSWORD --network=service_mysql redis:latest
    - # Set up your application and run tests
    - # Clean up the MySQL and Redis containers
    - docker stop mysql-container
    - docker rm mysql-container
    - docker stop redis-container
    - docker rm redis-container


```


