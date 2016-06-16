# CRS Testing

There are 3 instances of Kubernetes available for testing:

* **crs1** 20 nodes, master node 172.16.7.20
* **crs2** 10 nodes, master node 172.16.7.41
* **crs3** 10 nodes, master node 172.16.7.51

To start a new crs run login to the chosen master node (ex. `ssh cgc@172.16.7.20`) and:

```
virtualcompetition restart
crs start
```

to stop:

```
crs dump
crs stop
```

Every component update generates a new Docker image, but the CRSs are not automatically updated.
To use the latest images:

```
crs update
```

## Virtual Competition

CRS simulations use virtual-competition running on node 172.16.7.72.

Binaries are from the standard CGC sample set.

To add/remove binaries to the standard set (for **crs1** for example)
login into `172.16.7.72` and change files in
`virtual-competition-crs1/shared/cgc-challenges/`.


Binaries MUST have this directory structure:

```
CADET_00001
└── bin
    └── CADET_00001

MULTIBINARYSET_00001
└── bin
    ├── MULTIBINARYSET_00001_1
    └── MULTIBINARYSET_00001_2
```
