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
./setup.sh -p cgc -r git@github.com:mechaphish -r git@github.com:shellphish -D \
                ana idalink cooldict mulpyplexer monkeyhex superstruct \
                shellphish-afl shellphish-qemu capstone unicorn peewee \
            	archinfo vex pyvex cle claripy simuvex angr angr-management angr-doc \
                binaries identifier fidget angrop driller fuzzer tracer \
                compilerex povsim rex farnsworth patcherex colorguard \
                common-utils network_poll_creator patch_performance \
                worker meister ambassador scriba
```

Annotated:
- Use the **setup.sh** from the angr-dev repository to perform installation
- Create a **virtual environment** with the **pypy** interpreter, named "cgc"
- Use the **mechaphish** and **shellphish** github organizations for data sources in addition to the hardcoded defaults
- Specifies all python packages to install explicitly

Note that this will take a *VERY LONG TIME*, since installing shellphish-afl and shellphish-qemu involves building qemu about 40 times.
There is a binary distribution that should take significantly shorter, but there are distribution issues at present.

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

To run a virtual competition that serves challenge sets locally:

1. install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) and [Vagrant](https://www.vagrantup.com/downloads.html)
2. run

   ```
   git clone git@github.com:mechaphish/virtual-competition.git
   cd virtual-competition
   # put any challenge sets you want fielded in shared/cgc-challenges
   # the format is a folder for each challenge set, which contains a `bin` folder
   # which contains all the binaries for the challenge set
   vagrant up crs
   bin/launch start
   ```
