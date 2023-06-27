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

