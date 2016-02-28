# Development setup

## Locally

Run CRS manually without Kubernetes.

You need to customize .env file to match your setup.

```
git clone git@git.seclab.cs.ucsb.edu:cgc/common.git
git clone git@git.seclab.cs.ucsb.edu:cgc/farnsworth-client.git
git clone git@git.seclab.cs.ucsb.edu:cgc/farnsworth.git
git clone git@git.seclab.cs.ucsb.edu:cgc/worker.git
git clone git@git.seclab.cs.ucsb.edu:cgc/meister.git

pip install -e common
pip install -e farnsworth-client

# install Postgres
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main" >> /etc/apt/sources.list.d/postgresql.list'
sudo apt-get install postgresql-9.5

# connecto to VPN
# ask Yan for VPN scripts
cd team6/ && sudo openvpn team6-tcp-client.ovpn

# run Farnsworth
cd farnsworth
cp .env.development .env
psql < support/database/schema.sql
pip install -e .
python develop.py

# run Meister
cd meister
pip install -e .
cp .env.development .env
meister

# run worker
cp .env.development .env
JOB_ID=xxx worker
```
