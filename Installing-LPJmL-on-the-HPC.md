# Installing LPJmL on Anunna, the WUR HPC

## Downloading LPJmL
The official LPJmL repository is on GitHub: https://github.com/PIK-LPJmL/LPJmL

The code can be downloaded using the git command `clone`:
```
git clone https://github.com/PIK-LPJmL/LPJmL.git
```
A logical place to do this is in (a subdirectory of) your home directory, which can be accessed from all nodes on the cluster. Note you shouldn't store any data in your home directory, instead use [the lustre file system for storing datasets](https://wiki.anunna.wur.nl/index.php/Filesystems). 

## Compiling the model

