# Running the default LPJmL model run

To run LPJmL, we need to modify the configuration files. The three main files to modify are: `lpjml.conf` (for the general setup - settings, or start and stop year), `input.conf` (setup of regional input files like climate, or landuse patterns) and `param.conf` (global model parameters).

Here we attempt to adapt the default model configuration to our system. 

:information_source: Instead of modifying files in our copy of the official LPJmL repository, we can copy the files to a different directory and modify these copies. It may also be a good idea to put the configuration files that you use under version control, as these define all the settings used to run the model. You can use this WUR repository ([LPJmL_tools](https://git.wur.nl/danke010/lpjml_tools)) for this purpose. The configuration files for a standard run (driven by CRU climate data) have been copied from the official LPJmL repository (v4.0.002) and adapted for the setup on the WUR HPC.

Open the file `input.conf` in a text editor and change the location of the file `input_crumonthly.conf` specifying the input files for a default CRU run. You can find this in section `III` of the file:

```
/*===================================================================*/
/*  III. Input data section                                          */
/*===================================================================*/

#include "/home/WUR/danke010/mycode/lpjml_tools/conf/default_cru/input_crumonthly.conf"    /* Input files of CRU dataset */

```

We also need to change the location of the output files. This can be done in section `IV` of the file, but a trick is to define the output directory at the top of the configuration file by adding a line `#define output` like this:

```
...
#include "include/conf.h" /* include constant definitions */
#define output /lustre/scratch/WUR/ESG/danke010/LPJmL/output/cru/default

#define RIVER_ROUTING /*river routing enabled; exclude this line to disable river routing */
...
``` 

We can also change other settings such as the length of the spinup period and the type of output files we want, but we can leave the default settings for now. 

Finally, we change the file location of the (output) restart file that will be produced in section `V`. You could write to the standard output directory `output` defined above, but if you later want to start a new model run from this restart file, you would have to point to the same directory, or move the file. 

```
NO_RESTART /* do not start from restart file */
RESTART /* create restart file: the last year of simulation=restart-year */
output/restart_1840_nv_stdfire.lpj /* filename of restart file */
```

:warning: If we start a run from a restart file, we should also change the path and/or the filename of the (input) restart file in section `V`.

Next we open the file `input_crumonthly.conf` and change the paths of the relevant input files. Again, a trick to do this is to define an `inputdir` at the start of the file:

```
...
#include "include/conf.h" /* include constant definitions */
#undef LPJ
#define inputdir /lustre/scratch/WUR/ESG/danke010/CALM/LPJmL_input/input_VERSION2
...
```

We can then replace the default destination in the file from the standard repository (`/p/projects/lpjml/input/historical/input_VERSION2`) with `inputdir` throughout the file, like this:

```
...
//RAW inputdir/soil_new_67420.bin
META inputdir/soil.descr
CLM2 inputdir/grid.bin
#ifdef WITH_LANDUSE
...
```

We also need to specify the location of the climate input files, if these are different:

```
CLM2 /lustre/scratch/WUR/ESG/danke010/CALM/LPJmL_input/CRUDATA_TS3_23/cru_ts3.23.1901.2014.tmp.dat.clm
CLM2 /lustre/scratch/WUR/ESG/danke010/CALM/LPJmL_input/CRUDATA_TS3_23/gpcc_v7_cruts3_23_precip_1901_2013.clm
...
CLM2 /lustre/scratch/WUR/ESG/danke010/CALM/LPJmL_input/CRUDATA_TS3_23/cru_ts3.23.1901.2014.cld.dat.clm
...
CLM2 /lustre/scratch/WUR/ESG/danke010/CALM/LPJmL_input/CRUDATA_TS3_23/cru_ts3.23.1901.2014.dtr.dat.clm            /* diurnal temp. range */
...
CLM /data/biosx/mforkel/input_new/landcover_synmap_koeppen_vcf_newPFT_forLPJ_20130910.clm /*synmap_koeppen_vcf_NewPFT_adjustedByLanduse_SpinupTransitionPrescribed_forLPJ.clm*/
```

:information_source: The default `input_crumonthly.conf` makes use of `META` files, a small text file describing the actual binary input files. If you have copied the standard input files from elsewhere, you need to change the path in this description file. For example, the file `soil.descr` looks like this:

```
remark "soil code data file"
remark "valid soil codes are in the range 1..14"
file /p/projects/biodiversity/input_VERSION2/soil_new_67420.bin
firstcell 0
ncell 67420
datatype byte
cellsize 0.5 0.5
```

The safest way to do this is to provide the full path to where the binary file resides, as in the example above. 