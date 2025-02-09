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

There is a large suite of postprocessing and diagnostic packages developed by NCAR using a combination of NCL and Python scripts that automatically generate a variety of different kinds of plots from model output files and used to evaluate a simulation. They all compute a series of pre-defined metrics and display the plots via a website.

There are five main diagnostics packages:
1. Atmosphere
2. Ice
3. Land
4. Ocean
5. Climate Variability and Diagnostics Package (CVDP)


### Postprocessing and Diagnostics Packages Setup

We will setup everything necessary for you to be able to run the postprocessing and diagnostics packages on the NCAR computers.

#### Load the  postprocessing and diagnostics packages:

~~~
$ module use /glade/work/bdobbins/Software/Modules
$ module load cesm_postprocessing_derecho
~~~
{: .language-bash}

#### Set up the working directory to run the package:

Go to the directory of case to be processed:

~~~
$ cd /glade/derecho/scratch/cstan/run.2
~~~
{: .language-bash}

Create the case to be postprocessed:

~~~
$ create_postprocess -caseroot=/glade/derecho/scratch/cstan/run.2/postprocess
$ cd postprocess/
$ cp /glade/u/home/dbailey/timeseries .
~~~
{: .language-bash}

Edit timeseries:

- edit project number
- edit CASE (=casename)
- edit CASEROOT (=where the model data sits, but do not include the casename, e.g., /glade/derecho/scratch/cstan)

Edit env_postprocess.xml file:

- set CASEROOT and PP_CASE_PATH to the post-processing directory code location (ex. /glade/derecho/scratc/cstan/run.2/postprocess)
- check that CASE is set correctly
- set DOUT_S_ROOT correctly (if you don't recall this, use `xmlquery`  
- set TIMESERIES_INPUT_DIRECTORY and TIMESERIES_OUTPUT_ROOTDIR to where the output should be written (default is DOUT_S_ROOT/[compname]/proc/tseries)

~~~
cp -R /glade/u/home/asphilli/CESM_tutorial/* .
~~~
{: .language-bash}

You may get an error about being able to copy a particular file. You can ignore the error.

Put the configuration file into the correct location:

~~~
mv hluresfile ../.hluresfile
~~~
{: .language-bash}

#### Setup the python environment for the CESM diagnostics and post-processing scripts

~~~
cesm_pp_activate 
~~~
{: .language-bash}

#### Create a directory for the CESM postprocessing code:

~~~
mkdir /glade/derecho/scratch/cstan/cesm-postprocess
~~~
{: .language-bash}

#### Run the postprocessing using `create_postprocess` and tell it the name of your 4-year case

~~~
create_postprocess --caseroot /glade/derecho/scratch/cstan/cesm-postprocess/run.2
~~~
{: .language-bash}

#### Go to the postprocessing directory:

~~~
cd /glade/derecho/scratch/cstan/cesm-postprocess/run.2
~~~
{: .language-bash}

#### Set the location of the model data:

~~~
./pp_config --set DOUT_S_ROOT=/glade/derecho/scratch/cstan/archive/run.2
~~~
{: .language-bash}

#### Tell the diagnostics what kinds of grids to expect, our version uses:

~~~
./pp_config --set ATM_GRID=1.9x2.5
./pp_config --set LND_GRID=1.9x2.5
./pp_config --set ICE_GRID=gx1v7
./pp_config --set OCN_GRID=gx1v7
./pp_config --set ICE_NX=320
./pp_config --set ICE_NY=384
~~~
{: .language-bash}
