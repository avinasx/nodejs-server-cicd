# nodejs-server-cicd

How to set ci/cd for nodejs app with aws code deploy and aws code pipline

## Installation instructions

### 1. Launch ubuntu 20 server in aws

### 2. ssh to ubuntu 20 to install packages

```sh
ssh -i <key.pem> ubuntu@<ip-address> -v
```

### 3. Update and Upgrade ubuntu machine and install node, nvm and pm2

```sh
sudo apt-get update
```

```sh
sudo apt-get upgrade
```
#### 3.1 install node

To **install** or **update** nvm, you should run the [install script][2]. To do that, you may either download and run the script manually, or use the following cURL or Wget command:
```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```
Or
```sh
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

Running either of the above commands downloads a script and runs it. The script clones the nvm repository to `~/.nvm`, and attempts to add the source lines from the snippet below to the correct profile file (`~/.bash_profile`, `~/.zshrc`, `~/.profile`, or `~/.bashrc`).

#### 3.2 Copy & Past (each line separately)
<a id="profile_snippet"></a>
```sh
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

#### 3.3 Verify that nvm has been installed

```sh
nvm --version
```

#### 3.4 Install node

```sh
nvm install --lts # Latest stable node js server version
```

#### 3.5 Check nodejs installed
```sh
node --version
```

#### 3.6 Check npm installed
```sh
npm -v
```

### 4. Clone nodejs-server-cicd repository

```sh
cd /home/ubuntu
```

```sh
git clone https://github.com/saasscaleup/nodejs-server-cicd.git
```

### 5. Run node app.js  (Make sure everything working)

```sh
cd /home/ubuntu/nodejs-server-cicd
```

```sh
node app.js
```

### 6. Install pm2 and run as sudo (Run nodejs in background and when server restart)
```sh
npm install -g pm2 # may require sudo
```

#### 6.1 Starting the app
```sh
sudo pm2 start app.js --name=nodejs-server
```
```sh
sudo pm2 save     # saves the running processes
                  # if not saved, pm2 will forget
                  # the running apps on next boot
```

#### 6.2 IMPORTANT: If you want pm2 to start on system boot
```sh
sudo pm2 startup # starts pm2 on computer boot
```

### 7. Set node and pm2 available to root

```sh
sudo ln -s "$(which node)" /sbin/node
```
```sh
sudo ln -s "$(which npm)" /sbin/npm
```
```sh
sudo ln -s "$(which pm2)" /sbin/pm2
```

### 8. Install aws code deploy agent 
```sh
sudo yum install -y ruby
```

```sh
wget https://aws-codedeploy-us-east-1.s3.amazonaws.com/releases/codedeploy-agent_1.0-1.1597_all.deb
```
```sh
mkdir codedeploy-agent_1.0-1.1597_ubuntu20
```
```sh
dpkg-deb -R codedeploy-agent_1.0-1.1597_all.deb codedeploy-agent_1.0-1.1597_ubuntu20
```
```sh
sed 's/2.0/2.7/' -i ./codedeploy-agent_1.0-1.1597_ubuntu20/DEBIAN/control
```
```sh
dpkg-deb -b codedeploy-agent_1.0-1.1597_ubuntu20
```
```sh
sudo dpkg -i codedeploy-agent_1.0-1.1597_ubuntu20.deb
```
```sh
sudo systemctl start codedeploy-agent
```
```sh








## to auto pull using github action

do this on your server 

Store username and password in .git-credentials
.git-credentials is where your username and password (access token) is stored when you run git config --global credential.helper store, which is what other answers suggest, and then type in your username and password or access token:

https://${username}:${password_or_access_token}@github.com
So, in order to save the username and password (access token):

git config --global credential.helper store
echo "https://${username}:${password_or_access_token}@github.com" > ~/.git-credentials
Replace ${username} with your username, ${password_or_access_token} with your password (not recommended) or your access token.

NOTE that you must provide access token if you enabled 2FA on GitHub.

Using access token is recommended.

This is very useful for a GitHub robot, e.g. to solve Chain automated builds in the same Docker Hub repository by having rules for different branch and then trigger it by pushing to it in the post_push hook in Docker Hub.
sudo systemctl enable codedeploy-agent
```

### 9. Continue in AWS console...

Watch the rest of the youtube video...
