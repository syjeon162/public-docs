**Helpful Resource about using Singularity on the SCC: https://www.bu.edu/tech/support/research/software-and-programming/containers/singularity/**

Normally, running Singularity opens a virtual environment where you do not have access to your usual files and programs (ex. you won't have access to your home or project directories on the SCC). To gain access to certain directories, you need to mount (`-B`) them.

SCC provides a convenient wrapper for Singularity, which can be run using `scc-singularity`. This by default provides access to home/project directories as well as some environment variables.

# Using Singularity with RP2 / EosSims

## Running simulations on the command line

Singularity containers have been placed under `/projectnb/snoplus/Eos/containers`.

To run, simply do:
```
scc-singularity --nolocal run /projectnb/snoplus/Eos/containers/EosSims-v2.1.0.sif
```

This will put you into a virtual environment that has all dependencies (ROOT, Geant4), RP2, and EosSims installed.

## Submitting Batch Jobs

You will need to write a separate shell script with what you want to do inside the container. For instance, a `run_singularity.sh` file could contain

```
cd /path/to/dump_dir
eos /path/to/macro.mac
```

You have to check that you have execute permissions to this shell script file -- you may need to give yourself these permissions (`chmod u+x run_singularity.sh`).

Then, write a script to submit. For instance, `qsub.sh` could contain:

```
#!/bin/bash -l

#$ -P snoplus
#$ -N JOBNAME

#$ -l h_rt=00:30:00

#$ -j y
#$ -V

scc-singularity --nolocal run /projectnb/snoplus/Eos/containers/EosSims-v2.1.0.sif /path/to/run_singularity.sh
```

Then submit it as a batch job:
```
qsub qsub.sh
```

# Developing RP2 / EosSims with Singularity

To be updated…

# Building new containers

## Ratpac-two

Building a new RP2 container is easy. In general, follow steps specific to the SCC from [here](https://www.bu.edu/tech/support/research/software-and-programming/containers/building/) ("Building From Docker or Singularity Hub"), and specific to RP2 from [here](https://github.com/rat-pac/ratpac-two?tab=readme-ov-file#quick-start-with-containers).

Just remember to replace `apptainer` with `singularity`.

In the ratpac-two container, the setup script for ratpac-two is part of the runscript. (ie. things won't run if we use `singularity exec`.)

## EosSimulations

Similarly, make sure to follow steps specific to SCC ("Building From Scratch").

Make a `.def` file with the following content:

```
Bootstrap: docker
From: ratpac/ratpac-two:main

%setup
	git clone https://github.com/EosDemonstrator/EosSimulations.git ${SINGULARITY_ROOTFS}/EosSimulations

%post -c /bin/bash
	source /ratpac-setup/env.sh
	cd /EosSimulations
	cmake . -Bbuild -DCMAKE_INSTALL_PREFIX=$(pwd)/install
	cmake --build build -- -j$(nproc)
	cmake --install build

%environment
	source /EosSimulations/eos.sh
```

Then, run
```
singularity build --fakeroot EosSims-v0.0.0.sif /path/to/EosSimulations.def
```

Github will prompt you to login. You need to make a [Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) (**classic**, not fine-grained) and use that for your password instead of your usual login.
