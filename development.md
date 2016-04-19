# Development setup

How to run CRS manually without Kubernetes.

## Requirements

* angr
* docker (`curl -fsSL https://get.docker.com/ | sh`)


## Install

```
git clone git@git.seclab.cs.ucsb.edu:cgc/cbs.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/farnsworth.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/worker.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/meister.git && \
pip install -e farnsworth && \
pip install -e worker && \
pip install -e meister

# setup db
cd farnsworth
cp .env.example .env
# edit .env if needed
./setupdb.sh
```


## Run

```
# run Postgres
sudo docker run -p 127.0.0.1:5432:5432 -d postgres:9.5

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
