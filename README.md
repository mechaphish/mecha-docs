# The Mechanical Phish

The Mechanical Phish is open source!
Mechanical Phish was created by Shellphish as our CRS for the DARPA Cyber Grand Challenge.
It rocked it in the final event, winning 3rd place, and we are very proud of it.

The Cyber Grand Challenge was the first time anything like this was attempted in the security world.
As such, Mechanical Phish is an *extremely* complicated piece of software, with an absurd amount of components.
No blueprint for doing this existed before the CGC, so we had to figure things out as we went along.
Unfortunately, rather than being a software development shop, we are a "mysterious hacker collective".
This means that Mechanical Phish has some rough components, missing documentation, and ghosts in the machine.
Our hope is that, going forward, we can polish and extend Mechanical Phish, as a community, to continue to push the limits of automated hacking.

# Issues

So far, there are several glaring issues that came up during the runup to the CGC Final Event, the CFE itself, DEFCON, or our post-CGC analysis:

- There is very little documentation of the whole thing. This is something that we would love community involvement for (although it's admittedly a chicken-and-egg problem).
- Setting Mechanical Phish up can be an ordeal. A ready-built docker would be cool.
- There are probably lots of URLs pointing to our internal infrastructure. These will need to be changed to point to github as they're identified.
- There are some issues we're aware of:
 * Our Kubernetes setup has problems scheduling things quickly, leading to under-utilitization of the infrastructure.
 * An accidental assert, partway through our multi-CB exploitation pipeline, completely disables Mechanical Phish's ability to exploit multi-CB challenge sets.
 * There are some race conditions between the patch selection and patch submission, leading to too many patch submissions.
 * The scheduler for analyzing network traffic has a bug that causes it to pull down the full traffic rather than the metadata, essentially disabling that component after 15 rounds of the game.
 * There are other, mysterious scheduling issues that cause exploitable crashes to be overlooked (for example, during the CFE, Mechanical Phish identified 40 exploitable crashes, but only scheduled the generation of 15 exploits).

Overall, we're pretty surprised this thing ran at all!
In the development of Mechanical Phish, we had to fix some bugs in some underlying components.
We have fixes our fixes in the following forks, but we need to upstream them:

- [Unicorn Engine](https://github.com/angr/unicorn)
- [Capstone Engine](https://github.com/angr/capstone)
- [PeeWee](https://github.com/mechaphish/peewee)

# Components

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

## Other

There are also many other repositories that act as dependencies for those above.
We'll get a full list together at some point.

# Setup

We have documented the process to setup a local development version [here](development.md).

Scripts for setting up the full, kubernetes-powered distributed systems are [here](https://github.com/mechaphish/setup).

# Support

We plan to keep MechanicalPhish alive through our research and the support of the community.
However, we are a small group of PhD students, and don't have much time to give support, as our primary task is to do research, publish papers, and graduate.
Please understand this when creating support requests: if resolving a support question takes too much time, it will likely never be done.
To maximize the chance of a resolution, meet us half way and try exploring the issue *with* us :-)
