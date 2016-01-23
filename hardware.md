# Hardware

The shellcloud is composed of 64 nodes with each:
- 2x Intel Xeon Processor E5-2670 v2 (25M cache, 2.50GHz) (20 physical cores
  per machine)
- 256GB Memory, 16x 16GB Dimms, Hynix HMT42GR7BFR4C-RD
- 2x 1TB HDD, Seagate ST91000640NS (in raid1)

Overall:
- 1280 physical cores, 2560 cores with hyper-threading
- 16TB of memory
- 64TB of disk space

# TODO
Burn in the machines
- CPU (prime)
- Memory (memtest)
- Disk (bonnie++)
