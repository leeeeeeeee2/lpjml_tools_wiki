# Installing LPJmL on Anunna, the WUR HPC

[[_TOC_]]

## Downloading LPJmL
The official LPJmL repository is on GitHub: https://github.com/PIK-LPJmL/LPJmL

The code can be downloaded using the git command `clone`:
```
$ git clone https://github.com/PIK-LPJmL/LPJmL.git
```
A logical place to do this is in (a subdirectory of) your home directory, which can be accessed from all nodes on the cluster. Note you shouldn't store any data in your home directory, instead use [the lustre file system for storing datasets](https://wiki.anunna.wur.nl/index.php/Filesystems). 

You should get a directory `LPJmL` with several subdirectories (e.g., `config`, `bin`, `par`, etc.). This directory is your `LPJROOT` directory where the LPJmL code resides. After installing you can confirm the origin of the repository like this:

```
$ git remote show origin
```

which should show the URL of the original repository. 

:information_source: In the remainder of this guide I am working with version **4.0.002**.   You can find the version number in the file `VERSION` in the `LPJROOT` directory (`$ less VERSION` to see the contents of the file).

## Compiling the model

Some brief instructions can be found on the [LPJmL Wiki](https://github.com/PIK-LPJmL/LPJmL/wiki/HowTo). In particular it mentions:
>>>
There are several dependencies on standard libraries and compiler setting, please consult the configure.sh and the Makefile templates in the folder config and adjust these to your local setup.
>>>

the file `configure.sh` can be found in the `LPJROOT` directory. Open this file in a text editor (e.g., `nano`; if you are using the program MobaXterm to connect to the HPC, you can browse to the file and double-click on it to edit the file in MobaXterm's own text editor).

Configuration and compilation seems to work straight out of the box if you load some [environment modules](https://wiki.anunna.wur.nl/index.php/Environment_Modules) first:

```
$ module load slurm mpich/gcc/64/3.1.3
$ module load netcdf/gcc/64/4.3.3
$ module load udunits/gcc/64/2.2.25
```

Then run `configure.sh`:

```
$ ./configure.sh
Configuring LPJmL 4.0.002...
No input directory found, LPJINPATH has to be set
Operating system is Linux
MPICH found
Create executables with make all
Put . /home/WUR/danke010/LPJml/LPJmL/bin/lpj_paths.sh in your /home/WUR/danke010/.profile
```

And compile the model and all the utilities programmes:

```
$ make all
```

This will print a lot of output to the screen, but if there are no error message, we can assume that compilation was successful. 


You can set the `$LPJROOT` environment variable like this:

```
$ export LPJROOT="/home/WUR/danke010/LPJml_v4.0.002/LPJmL"
```

We can confirm that the model has compiled by trying to run it:

```
$ $LPJROOT/bin/lpjml
```

If the model has compiled successfully, it will show the name of the model on the screen, but then fail with an error, as it is trying to read the default configuration file `lpjml.conf` which we haven't changed yet.

## Running the model

To run LPJmL, we need to modify the configuration files. The three main files to modify are: `lpjml.conf` (for the general setup - settings, or start and stop year), `input.conf` (setup of regional input files like climate, or landuse patterns) and `param.conf` (global model parameters).

Here we attempt to adapt the default model configuration to our system. 

:information_source: Instead of modifying files in our copy of the official LPJmL repository, we can copy the files to a different directory and modify these copies. It may also be a good idea to put the configuration files that you use under version control, as these define all the settings used to run the model. You can use this WUR repository ([LPJmL_tools](https://git.wur.nl/danke010/lpjml_tools)) for this purpose. The configuration files for a standard run (driven by CRU climate data) have been copied from the official LPJmL repository (v4.0.002) and adapted for the setup on the WUR HPC.