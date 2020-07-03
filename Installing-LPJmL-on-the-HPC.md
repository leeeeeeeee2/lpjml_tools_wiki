# Installing LPJmL on Anunna, the WUR HPC

## Downloading LPJmL
The official LPJmL repository is on GitHub: https://github.com/PIK-LPJmL/LPJmL

The code can be downloaded using the git command `clone`:
```
git clone https://github.com/PIK-LPJmL/LPJmL.git
```
A logical place to do this is in (a subdirectory of) your home directory, which can be accessed from all nodes on the cluster. Note you shouldn't store any data in your home directory, instead use [the lustre file system for storing datasets](https://wiki.anunna.wur.nl/index.php/Filesystems). 

You should get a directory `LPJmL` with several subdirectories (e.g., `config`, `bin`, `par`, etc.). This directory is your `LPJROOT` directory where the LPJmL code resides. After installing you can confirm the origin of the repository like this:

```
git remote show origin
```

which should show the URL of the original repository. 

## Compiling the model

Some brief instructions can be found on the [LPJmL Wiki](https://github.com/PIK-LPJmL/LPJmL/wiki/HowTo). In particular it mentions:
>>>
There are several dependencies on standard libraries and compiler setting, please consult the configure.sh and the Makefile templates in the folder config and adjust these to your local setup.
>>>

How to get this to work on the WUR HPC? 

the file `configure.sh` can be found in the `LPJROOT` directory. Open this file in a text editor (e.g., `nano`; if you are using the program MobaXterm to connect to the HPC, you can browse to the file and double-click on it to edit the file in MobaXterm's own text editor).
