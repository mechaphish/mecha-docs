# Development setup

How to run CRS locally (without Kubernetes).

## Requirements

* docker
* ubuntu 16.04
* the following packages: virtualenvwrapper python2.7-dev build-essential sudo libxml2-dev libxslt1-dev git libffi-dev cmake libreadline-dev libtool debootstrap debian-archive-keyring libglib2.0-dev libpixman-1-dev libpq-dev python-dev libc6:i386 libncurses5:i386 libstdc++6:i386 zlib1g:i386 pkg-config zlib1g-dev libtool libtool-bin wget automake autoconf coreutils bison libacl1-dev qemu-user qemu-kvm socat postgresql-client nasm binutils-multiarch llvm clang


## Install the CRS

This is a bit hacky, but it pays the bills:

```
git clone git@github.com:angr/angr-dev
cd angr-dev
./setup.sh -e cgc -r git@github.com:mechaphish -r git@github.com:shellphish -D \
                ana idalink cooldict mulpyplexer monkeyhex superstruct \
                shellphish-afl shellphish-qemu capstone unicorn peewee \
            	archinfo vex pyvex cle claripy simuvex angr angr-management angr-doc \
                binaries identifier fidget angrop tracer fuzzer driller \
                compilerex povsim rex farnsworth patcherex colorguard \
                common-utils network_poll_creator patch_performance \
                worker meister ambassador scriba virtual-competition manual-interaction
```

Annotated:
- Use the **setup.sh** from the angr-dev repository to perform installation
- Create a **virtual environment** named "cgc"
- Use the **mechaphish** and **shellphish** github organizations for data sources in addition to the hardcoded defaults
- Specifies all python packages to install explicitly

Note that this will take a *VERY LONG TIME*, since installing shellphish-afl and shellphish-qemu involves building qemu about 40 times.
There is a binary distribution that should take significantly shorter, but there are distribution issues at present.

Additionlly note that there's some sort of bug that prevents meister from running under pypy.
During the CGC we had the worker running with pypy and everything else under cpython.

## Run the CRS

### Run the database

Mechanical Phish needs postgres to run.
You can easily spawn it with docker:

```bash
sudo docker run -p 127.0.0.1:5432:5432 -d postgres:9.5
```

Now, you need to set up the DB itself:

```bash
workon cgc
cd farnsworth
cp .env.example .env
# edit .env if needed
./setupdb.sh
```

### Run meister

```bash
workon cgc
cd meister
cp .env.example .env
# edit .env if needed
meister
```

You *probably* want to change the environment to tweak the following options:

- Change `MEISTER_OVERPROVISIONING` to the fraction of your total system resources the CRS should be alowed to use.
  It is set to more than 1 because when running behind kubernetes, the kuernetes scheduler makes scheduling decisions of its own.
- Change `MEISTER_LOG_LEVEL` to a lower level, probably `INFO`.
  The default `DEBUG` is horrifyingly verbose.
- Change `MEISTER_NUM_THREADS` to a very small number.
  1 works most of the time.
  More can overwhelm the database unless you are working with a lot of compute power.

### Run Virtual Competition

To run a virtual competition that serves challenge sets locally, DARPA provided a set of scripts to serve as an API test mock.
We expanded it into a slightly more useful test mock.

The competition requires DARPA's DECREE VM.
To run it, you'll need to install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) and [Vagrant](https://www.vagrantup.com/downloads.html)

Then, to set up and run the vitual competition, run:

```bash
cd virtual-competition
vagrant up ti
bin/launch reset
bin/launch start
```

This will automatically compile and enable several sample CGC challenges.
If you would like to field your own challenges, place them in the `shared/cgc-challenges-unfielded` folder.
The behavior of the virtual competition with respect to fielding challenges is as follows:

- Rounds are 5 minutes long. The inter-round period is not emulated.
- At the start of a round where round % 10 == 0, cycle the challenge sets if there are any left in the queue.
  This means moving every challenge from `shared/cgc-challenges` to `shared/cgc-challenges-spent` and moving between 3 and 8 challenges from `shared/cgc-challenges-unfielded` to `shared/cgc-challenges`.
  Keep in mind that this does not happen if `shared/cgc-challenges-unfielded` is empty.
- Serve to the CRS any challenges in `shared/cgc-challenges`

### Run the Ambassador

The CRS still can't see any of these challenges!
The component that interacts with the CGC API is the ambassador.
Run it as follows:

```bash
workon cgc
cd ambassador
cp .env.example .env
# edit .env if you need
ambassador
```

### It's live!

The CRS should now be scheduling jobs.
You can jump into the database to look at the internal setup at any time:
`psql -hlocalhost -Upostgres farnsworth` should do the trick.

If you look at the `jobs` table (`select * from jobs`), you can see that there are some jobs to be done now!
However, because we're not running with kubernetes, none of these will actaully get run unless you run them manually:

```bash
workon cgc
cd worker
cp .env.development .env
# edit .env if needed
JOB_ID=xxx worker
```

This should run the job.
Each job syncronizes its own data and status into the database, so if your job produces interesting stuff, you should see more interesting jobs!
