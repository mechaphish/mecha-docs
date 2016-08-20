# Development setup

How to run CRS locally (without Kubernetes).

## Requirements

* docker
* ubuntu 16.04
* the following packages: virtualenvwrapper python2.7-dev build-essential sudo libxml2-dev libxslt1-dev git libffi-dev cmake libreadline-dev libtool debootstrap debian-archive-keyring libglib2.0-dev libpixman-1-dev libpq-dev python-dev libc6:i386 libncurses5:i386 libstdc++6:i386 zlib1g:i386 pkg-config zlib1g-dev libtool libtool-bin wget automake autoconf coreutils bison libacl1-dev qemu-user qemu-kvm socat postgresql-client nasm binutils-multiarch llvm clang


## Install

This is a bit hacky, but it pays the bills:

```
git clone git@github.com:angr/angr-dev
cd angr-dev
./setup.sh -i -w -p cgc -r git@git.seclab.cs.ucsb.edu:cgc -D \
    		ana idalink cooldict mulpyplexer monkeyhex superstruct \
        	capstone unicorn \
            	archinfo vex pyvex cle claripy simuvex angr angr-management angr-doc \
                binaries binaries-private identifier fidget angrop pwnrex driller fuzzer tracer \
                compilerex povsim rex farnsworth patcherex colorguard top-secret \
                common-utils network_poll_creator \
                worker meister ambassador scriba
```


## Run

Mechanical Phish needs postgres to run.
You can easily spawn it with docker:

```
sudo docker run -p 127.0.0.1:5432:5432 -d postgres:9.5
```

Now, you need to set up the DB itself:

```
workon cgc
cd farnsworth
cp .env.example .env
# edit .env if needed
./setupdb.sh
```

Run meister:

```
workon cgc
cd meister
cp .env.development .env
# edit .env if needed
meister
```

You'll now need to inject some challenge sets and so on, so that meister starts launching jobs (TODO: document this).
Then, as meister creates jobs, you can run them with:

```
workon cgc
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
