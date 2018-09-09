# Hyperledger Customer Loyalty Program Demo

* Reference
  * https://developer.ibm.com/code/patterns/customer-loyalty-program-with-blockchain/
  * https://github.com/IBM/customer-loyalty-program

## VM Preparation
* Ubuntu 16.04 LTS on VirtualBox (5.2.12)
  * 4096 MB Memory
  * 20GB Drive (10GB is NOT enough)
  * Take a snapshot in VirtualBox for restore once baseline OS is installed
  * SSH to Ubuntu VM using NAT & Port Forwarding
    * Virbualbox Settings > Network > Advanced > Port Forwarding
      * Host IP: 127.0.0.1
      * Host Port: 2222
      * Guest IP: 10.0.2.15
      * Guest Port: 22

## Ubuntu 16.04

`sudo apt-get install -y openssh-server`

## From MAC terminal

`ssh -p 2222 username@127.0.0.1`

## Pre-requisite

```
sudo apt-get install -y git

git clone https://github.com/IBM/customer-loyalty-program

sudo apt-get install -y npm

sudo apt install curl

curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -

sudo apt-get install -y nodejs

pwd
/home/<username>/
npm config set prefix /home/<username>/
```
 
## Hyperledger Composer Install (Composer Version : 0.19.4)

npm install -g composer-cli@0.19.4

npm install -g composer-rest-server@0.19.4

npm install -g generator-hyperledger-composer@0.19.4

* Note
Hyperledger Composer
https://stackoverflow.com/questions/48866375/errorunable-to-install-composer-cli

## Deploy Network - Deploy Hyperledger Fabric locally

### Install Docker CE

```
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

sudo apt-get update

sudo apt-get install -y docker-ce
```

* docker install in Ubuntu: https://docs.docker.com/install/linux/docker-ce/ubuntu/#set-up-the-repository

### Install docker-compose

```
sudo curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

* docker-compose install in Ubuntu: https://docs.docker.com/compose/install/#prerequisites

### demo Set-up

```
cd customer-loyalty-program/fabric-dev-servers

cd /home/<username>/customer-loyalty-program/fabric-dev-servers

sudo ./downloadFabric.sh

sudo ./startFabric.sh

./createPeerAdminCard.sh
```

### Run Application

```
cd customer-loyalty-program/

npm install

cd customer-loyalty-program

composer network install --card PeerAdmin@hlfv1 --archiveFile clp-network@0.0.1.bna

composer network start --networkName clp-network --networkVersion 0.0.1 --networkAdmin admin --networkAdminEnrollSecret adminpw --card PeerAdmin@hlfv1 --file networkadmin.card

composer network ping --card admin@clp-network

cd fabric-dev-servers

sudo ./startFabric.sh
```

```
cd ..

cd web-app

npm start
```
