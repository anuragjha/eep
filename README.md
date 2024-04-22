# Energy Exchange Platform
Allows exchange energy between the householders (peers)

## Overview
### Each instance of powerflow contains the following:
1. edgex services for a powerflow instance
2. trader service to trade and conclude a transaction
3. blockchain service to maintain chain

## Trader service
Trader service is the main service. 
It relates to a identity that is participitating in market.

Every trader service will run a blockchain service 
(each blockchain instance will act as a Ledger of transactions in the market).

## Blockchain service
Blockchain is proxy for "view of market". 
It is a Ledger of the powerflow market.

To begin participating in the powerflow network they have to register as miner.

Blockchain service sends a request to registration server.

And Register server responds with peerlist of other blockchain holders 
on the network.

Then download the current state of chain from any of the fellow blockchain holder.
 
## Register service for Blockchain
Register service keeps track of nodes in the network.
1. Registration service Nodes in network
Registration service node begins by calling /registerSelfTo api.
2. Blockchain service Nodes in network
Blockchain service sends a request "register/blockchain" to a Register service.
And the register service responds with list of other nodes in the network.
This act as start point of execution, for every blockchain service.


##Run details
run command

``go run main.go <ip> <port> <label>``

example 

``go run main.go 127.0.0.1 6686 temper``

### How to initialze this application
1. Run Edgex devices
1. Run atleast one registration service
2. For a actor in marketplace (each actor/trader controls 1 EdgeX instance)
    - a. Run a trader service 
    - b. Run a blockchain service for this trader service (register req to a register service)


## Setup Docker on Raspberry 

```linux
curl -sSL https://get.docker.com | sh
docker run --rm hello-world
```

If permission error is observed, run the following options:

**Option #1**
```linux
sudo docker run --rm hello-world
```
**Option #2**
```linux
usermod -aG docker <your_user>
```

**Install Python modules:**
```linux
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && sudo python3 get-pip.py
sudo apt-get install libssl-dev
sudo apt-get install libffi-dev
```
**Install vim:**
```linux
sudo apt-get update && sudo apt-get install vim -y
```

**Install docker-compose:**
```linux
sudo pip3 install docker-compose
vim docker-compose.yml
```
You can add your device in docke-compose.yml file.

```linux
version: '3'
services:
  webapp:
    ports:
      - 5000:8000
    image: python:3.7-alpine
    command: "python -m http.server 8000"
```
**Run the docker compose:**

```linux
sudo docker-compose up -d
sudo docker-compose down
```

## Dependancy

1. Goland IDE environment
2. Git 
3. EdgeX Foundary 


## Import Device-sdk-go

```linux
mkdir -p ~/go/src/github.com/edgexfoundry
cd ~/go/src/github.com/edgexfoundry
git clone https://github.com/edgexfoundry/device-sdk-go.git
mkdir device-simple
cp -rf ./device-sdk-go/example/* ./device-simple/
cp ./device-sdk-go/Makefile ./device-simple
cp ./device-sdk-go/Version ./device-simple/
cp ./device-sdk-go/version.go ./device-simple/

```

## Configure Device Service 

1.Main.go: 

import library

```linux
"github.com/edgexfoundary/device-simple/driver"
```
2. Configure Makefile:

```linux
MICROSERVICES=cmd/device-simple/device-simple
GOFLAGS=-ldflags "-X github.com/edgexfoundry/device-simple.Version=$(VERSION)"
cmd/device-simple/device-simple:
$(GO) build $(GOFLAGS) -o $@ ./cmd/device-simple
```
3. go.mod

```linux
GO111MODULE=on go mod init
```
Alternatively, the go.mod from existing SDK device can be used in simple device example, directory root on go.mod file should be updated.

## Build the Project

Make file is used to build the project. Build command in the build directory is:
```linux
make build
```
## Run the Project

Run the binery generated in the follwing directory, in this example simple-device

```linux
cd cmd
cd simple-device
./simple-device
```
## Kill or Delete Opened Containers

For re-run the application, it is required to kill or delete the devices that is running in docker background to avoid panic error in docker while is running to do so, do the following:

**Delete Using Postman** 

```linux
DELETE clocalhost:48081/api/v1/device/id/DEVICE_ID
```
```linux
docker container ls -a
docker container stop $(docker container ls -aq)
docker container rm $(docker container ls -aq)
```
while docker-compose down, we can run in with --remove-orphans

```linux
docker-compose up -d
docker-compose down --remove-orphans
```

## Run 

1. Docker Up
2. Make Build
3. Run Device Simple
4. Run Task Manager
5. Brows HTML template
6. Postman to run register device
7. Postman to make decision
9. Update Browser to update the page
