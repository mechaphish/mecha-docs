# Shellphish CRS

## Physical Nodes

The Shellphish CRS requires the following software installed on the physical
nodes:
- Docker for running jobs, such as AFL, Driller, etc.
- Kubernetes for scheduling Docker images
- GlusterFS for the distributed filesystem, which is being used by PostgreSQL


## Components

- **Meister**: Communicates with the CGC API and coordinates the workers.
- **Worker**: Runs various jobs based on meister commands.
- **Postgres**: Knowledge base of the CRS

## Additional components (for testing)

- **virtual-competition**: simulates CGC TI API
- **registry**: automatic update Docker images
