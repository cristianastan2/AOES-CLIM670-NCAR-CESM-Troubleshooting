---
title: "History and Setup for Diagnostics"
teaching: 0
exercises: 0 
questions:
- "What is in our history files?"
- "How do I setup to run the postprocessing and diagnostics packages"
objectives:
keypoints:
---

We will now return to the output from our 4-year case.  Let's go to the atmospheric history directory for our case.
If your 4-year case did not run to completion, you are welcome to look at mine.

~~~
cd /glade/derecho/scratch/cstan/archive/run.2/atm/hist
~~~
{: .language-bash}

### History vs. Timeseries Files

History files
: contain all of the variables for a componenet for a particular frequency and are output directly by the model.  

Timeseries files
: usually span a number of timesteps and contain only one major variable.  They are created offline.

When NCAR provides output from their model simulations publicly, they typically provide timeseries files for a select set of variables. 

Examples:

A history file: `f40_test.cam.h0.1993-11.nc`
* 1 monthly timestep (Nov 1993)
* 200+ CAM varaibles

A timeseries file: `f40_test.cam.h0.PSL.199001-199912.nc`
* 120 monthly timesteps (Jan 1990-Dec1999)
* 1 CAM variable (PSL), along with coordinate variables like `time`,`lat`,'lon`,etc.


### CESM Time Variable

The time coordinate variable in CESM history and timeseries files represents *the end* of the averaging period for variables that are averages.  The time that gets resolved when the data are read in does not match the date in the filename. For monthly averaged data, the filename is correct.  This can be a source of much confusion.

Example: `run.2.cam.h0.0001-05.nc`
* This is a history file for may of year one of our run.
* When you read in this file, the first time is resolved as: `0001-06-01`.  This means that Jun 1 of year 0001 is the end of the averaging period and the data contains the average for May of year 1.
* To verify the averaging period in the files, consult the `time_bnds`, `time_bound`, or `time_bounds` variable. Always check!!!!
* This is a convention used by CESM to allow averaged and instantaneous variables to be stored in the same file.


### Postprocessing 

The process of going from `history` files to `timeseries` files and to convert 3D atmospheric data from the model coordinate system to selected pressure levels. We will learn how to use the CESM Postprocessing Tools which are primarily written in NCAR Command Language (NCL) and PyNGL the Python version of NCL. We will use the preprepared NCL scripts without having to know too much NCL.


### Diagnostics Packages

There is a large suite of postprocessing and diagnostic packages developed by NCAR using Python scripts that automatically generate a variety of different kinds of plots from model output files and used to evaluate a simulation. They all compute a series of pre-defined metrics and display the plots via a website. These packages are under development. 

There are two main diagnostics packages:
1. [The Atmosphere Model Working Group (AMWG) Diagnostics Framework (ADF)](https://ncar.github.io/CESM-Tutorial/notebooks/diagnostics/additional/adf.html)
   * Climate Variability and Diagnostics Package (CVDP) 
3. [CESM Unified Postprocessing and Diagnostics (CUPiD)](https://www.cesm.ucar.edu/sites/default/files/2024-03/2024-cesm-sewg-tkingmlevypptx.pdf)
   * ADF
   * Climate Variability and Diagnostics Package (CVDP)

### Postprocessing and Diagnostics Packages Setup

We will setup everything necessary for you to be able to run the postprocessing and diagnostics packages on the NCAR computers. We will work on Casper, the system system of specialized data analysis and visualization resources; large-memory, multi-GPU nodes; and high-throughput computing nodes.

#### Login to Casper:

~~~
$ aah -XY username@casper.hpc.ucar.edu
~~~
{: .language-bash}

After running the `ssh` command, you will be asked to finish loggin in. 
Casper has full access to glade/

#### Checkout ADF and activate the conda environment:
~~~
$ git clone --recursive https://github.com/NCAR/ADF.git
$ module load conda
$ conda activate npl-2024a
~~~
{: .language-bash}

Also, along with these python requirements, the ncrcat NetCDF Operator (NCO) is also needed. This can be loaded by simply running:
~~~
$ module load nco
$ module load ncl
~~~
{: .language-bash}

### Configuration files:
The ADF requires 2 different yaml configuration files:

`config_amwg_default_plots.yaml` and `adf_variable_defaults.yaml`

> ## Do not modify either of these files!
> 
> It is recommended to make a copy of each file, make modifications in those copies, and then run them with the ADF.
>
{: .callout}

~~~
$ cd ADF
~~~
{: .language-bash}

## Run-time yaml

`config_amwg_default_plots.yaml`

This is the most important file for the ADF, it stores all the necessary information that the ADF needs to run including all the relevant information about the case and baseline/observation/cmip runs.

Make a copy of tis file that you will edit

~~~
$ cp config_amwg_default_plots.yaml config_amwg_myCopy_plots.yaml
~~~
{: .language-bash}

Open that copied file and the main sections you will want to change are:

user - use your NCAR ursename

compare_obs - set true if you want to compare your run with observations or false if you want to compare two runs

hist_str - [cam.h0, cam.h1]

cam_case_name - the name of the case run (no path included)

cam_hist_loc - where the h# history files live (example for my run.2 case: /glade/derecho/scratch/cstan/archive/${diag_cam_climo.cam_case_name}/atm/hist)

start/end_years - climo years desired 



