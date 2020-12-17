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

This will create a file `Makefile.inc` in the `LPJROOT` directory. You can try compiling the model with the `make` command (see below), but if we want to compile with support for netcdf files, we need to edit the makefile first. Some further details on all the compiling options can be found [on this page](http://www.pik-potsdam.de/~bloh/lpjml/install.html). First we need to add the flags for compiling with netcdf enabled:

```
LPJFLAGS  = -DUSE_RAND48 -DUSE_MPI -DSAFE -DWITH_FPE -DUSE_NETCDF -DUSE_UDUNITS -DUSE_NETCDF4 
```

We also need to explicitly set the paths to the netcdf shared libraries:

```
NETCDF_INC      = -I/cm/shared/apps/netcdf/gcc/64/4.3.3/include -I/cm/shared/apps/udunits/gcc/64/2.2.25/include
NETCDF_LIB      = /cm/shared/apps/netcdf/gcc/64/4.3.3/lib
UDUNITS_LIB      = /cm/shared/apps/udunits/gcc/64/2.2.25/lib
```
Finally, change the line with `LIBS = ` into

```
LIBS    = -lm -L$(NETCDF_LIB) -lnetcdf -L$(UDUNITS_LIB) -ludunits2
```

An example makefile that appears to work on Anunna has been [added to the repository](https://git.wur.nl/danke010/lpjml_tools/-/blob/master/Makefile.inc).

Once the makefile has been edited we can compile the model and all the utilities programmes:

```
$ make all
```

This will print a lot of output to the screen, but if there are no error message, we can assume that compilation was successful. If we need to undo (or redo) the compilation, we need to use the command `make clean` first.


You can set the `$LPJROOT` environment variable like this:

```
$ export LPJROOT="/home/WUR/danke010/LPJml_v4.0.002/LPJmL"
```

We can confirm that the model has compiled by trying to run it:

```
$ $LPJROOT/bin/lpjml
```

If the model has compiled successfully, it will show the name of the model on the screen, but then fail with an error, as it is trying to read the default configuration file `lpjml.conf` which we haven't changed yet.

Next we can try to run the model, see [Running a default LPJmL run](https://git.wur.nl/danke010/lpjml_tools/-/wikis/Running-a-default-LPJmL-run).