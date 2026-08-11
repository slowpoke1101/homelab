## Description
Steps to install docker on linux

## Purpose
for new VM deployments

##
Step 1: install docker
```
#install dependencies
sudo apt update
sudo apt install ca-certificates curl gnupg
```

Step 2: add dockers official gpg key
```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Step 3: add docker repo, first find os version codename
```
cat /etc/os-release #find version codename and add to the end of URL
#in my case ubuntu jammy
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/<ADD VERSION CODENAME HERE> stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Step4: update new repository and install 
```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
# enable on boot
sudo systemctl enable docker
sudo systemctl start docker
```

Step 5(optional): add user to docker group to avoid having to type sudo
```
sudo usermod -aG docker <username>
# NEED TO LOGOUT for changes to take effect lol this tripped me up
```