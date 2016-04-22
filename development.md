# Development setup

How to run CRS locally (without Kubernetes).

## Requirements

* angr
* docker (`curl -fsSL https://get.docker.com/ | sh`)
* openvpn (`sudo apt-get install openvpn`)


## Install

```
git clone git@git.seclab.cs.ucsb.edu:cgc/cbs.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/farnsworth.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/worker.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/meister.git && \
pip install -e farnsworth && \
pip install -e worker && \
pip install -e meister
```


## Run

```
# run Postgres
sudo docker run -p 127.0.0.1:5432:5432 -d postgres:9.5

# setup db
cd farnsworth
cp .env.example .env
# edit .env if needed
./setupdb.sh

# connecto to VPN for virtual-competition (ask Yan for VPN scripts)
cd team6/ && sudo openvpn team6-tcp-client.ovpn

# run Meister
cd meister
cp .env.development .env
# edit .env if needed
meister

# run worker
cd worker
cp .env.development .env
# edit .env if needed
JOB_ID=xxx worker
```


## Run Virtual Competition

You can use the virtual competition running on CGC node `172.16.7.19:8888`.
If you want to run virtual competition locally:

1. install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) and [Vagrant](https://www.vagrantup.com/downloads.html)
2. run

   ```
   git clone git@git.seclab.cs.ucsb.edu:cgc/virtual-competition.git && \
   git clone git@git.seclab.cs.ucsb.edu:cgc/tester.git && \
   cd virtual-competition && \
   vagrant up && \
   vagrant ssh ti -c "/vagrant/bin/launch start"
   ```
