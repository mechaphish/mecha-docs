# Repositories

The CRS has a *lot* of moving parts.
This is an index of all of them, split by component.

## Meister

The CRS Meister handles task scheduling.

- [Meister](https://github.com/mechaphish/meister)

## Ambassador

The Ambassador talks to the CGC API to retrieve CBs, submit POVs, etc.

- [Ambassador](https://github.com/mechaphish/ambassador

## Scriba

Scriba decides what exploits and RCBs to submit.

- [Scriba](https://github.com/mechaphish/scriba

## Worker

The CRS worker handles running the actual tasks that are scheduled by the worker.
This part includes a lot of repositories working together.

- [Worker](https://github.com/mechaphish/worker) - the actual glue
- [Rex](https://github.com/shellphish/rex) - Shellphish's automated exploitation component
- [Patcherex](https://github.com/shellphish/patcherex) - Shellphish's atomatic patching engine.
- [Fuzzer](https://github.com/shellphish/fuzzer) - a Python wrapper around AFL, and the fuzzer component of the Shellphish CRS.
- [Tracer](https://github.com/angr/tracer) - Component that traces CGC binaries symbolically with concrete inputs (convenience wrapper for concolic tracing).
- [Driller](https://github.com/shellphish/driller) - The Shellphish CRS "smart fuzzer".
- [Shellphish-QEMU](https://github.com/angr/shellphish-qemu) - A pip wrapper around our ridiculous amounts of qemu ports.
- [Shellphish-AFL](https://github.com/shellphish/shellphish-afl) - A pip wrapper to easily distribute AFL.

## Common

There are several common components that don't fit in a single category above.

- [Farnsworth](https://github.com/mechaphish/farnsworth) - Farnsworth is the knowledge base of the Shellphish CRS. It provides a JSON-based REST API and uses PostgreSQL as the data store.
- [Common-utils](https://github.com/mechaphish/common-utils) - Common utilities that didn't fit elsewhere.
- [Setup](https://github.com/mechaphish/setup) - Setup scripts to assist in CRS deployment.
