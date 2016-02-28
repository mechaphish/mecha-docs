# Development setup

## Locally

Run CRS manually without Kubernetes.

You need to customize .env file to match your setup.

```
git clone git@git.seclab.cs.ucsb.edu:cgc/farnsworth-client.git
git clone git@git.seclab.cs.ucsb.edu:cgc/farnsworth.git
git clone git@git.seclab.cs.ucsb.edu:cgc/worker.git
git clone git@git.seclab.cs.ucsb.edu:cgc/meister.git

pip install -e farnsworth-client

# run Farnsworth
sudo apt-get install postgresql-9.5
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
