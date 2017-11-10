+++
title = "Meteor Deployment to Digital Ocean"
date = 2017-11-09T15:10:17+01:00
weight= 1
+++

### set some values
```
  username = 'yourusername'
  appname = 'yourappname'

```

### create new user

```
# adduser $username
```

### add root privileges

```
usermod -aG sudo $username
```


### add public key for new user

```
sudo mkdir -p ~$username/.ssh
touch $HOME/.ssh/authorized_keys
sudo sh -c "cat $HOME/.ssh/authorized_keys >> ~$username/.ssh/authorized_keys"
sudo chown -R $username: ~$username/.ssh
sudo chmod 700 ~$username/.ssh
sudo sh -c "chmod 600 ~$username/.ssh/*"sudo mkdir -p ~$username/.ssh
```

### install some dependencies
```
apt-get install build-essential
apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev
```

### install python 2.7 for meteor
```
version=2.7
mkdir ~/Downloads && cd ~/Downloads
wget https://www.python.org/ftp/python/$version/Python-$version.tgz


#Extract and go to the directory
tar -xvf Python-$version.tgz
cd Python-$version

#install python
./configure
make
make install

```



### install nodejs
```
sudo apt-get update

sudo apt-get install -y curl apt-transport-https ca-certificates &&
  curl --fail -ssL -o setup-nodejs https://deb.nodesource.com/setup_4.x &&
  sudo bash setup-nodejs &&
  sudo apt-get install -y nodejs build-essential
```



#Installing Passenger + Nginx on a


sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo apt-get install -y apt-transport-https ca-certificates

#### Add our APT repository
```
sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger xenial main > /etc/apt/sources.list.d/passenger.list'
sudo apt-get update
```

### Install Passenger + Nginx
```
sudo apt-get install -y nginx-extras passenger
```

####  enable the Passenger Nginx module and restart Nginx
```
nano /etc/nginx/nginx.conf
```

#### uncomment include /etc/nginx/passenger.conf;
```
include /etc/nginx/passenger.conf;

sudo service nginx restart
```

#### check installation
sudo /usr/bin/passenger-config validate-install

sudo /usr/sbin/passenger-memory-stats


#### update regularly
```
sudo apt-get update
sudo apt-get upgrade
```




#### local
```
meteor build --architecture os.linux.x86_64 --server-only ../new_package && mv ../new_package/*.tar.gz package.tar.gz

scp package.tar.gz $username@serverIP:
```

#### server
```
sudo mkdir -p /var/www/$appname

cd /var/www/$appname
tar xzf ~/package.tar.gz
chown -R $username: .


cd /var/www/$appname/bundle/programs/server
npm install --production

sudo nano /etc/nginx/sites-enabled/$appname.conf
```
