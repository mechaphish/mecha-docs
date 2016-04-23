# Development setup

How to run CRS locally (without Kubernetes).

## Requirements

* Python 2.7 with virtualenv
* docker (`curl -fsSL https://get.docker.com/ | sh`)
* openvpn (`sudo apt-get install openvpn`)


## Install

```
mkvirtualenv cgc
workon cgc

git clone git@git.seclab.cs.ucsb.edu:cgc/fuzzer.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/rex.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/patcherex.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/cbs.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/farnsworth.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/worker.git && \
git clone git@git.seclab.cs.ucsb.edu:cgc/meister.git && \
pip install -e fuzzer && \
pip install -e rex && \
pip install -e patcherex && \
pip install -e farnsworth && \
pip install -e worker && \
pip install -r meister/requirements.txt && \
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
