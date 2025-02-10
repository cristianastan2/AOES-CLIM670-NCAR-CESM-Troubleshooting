---
title: "Postprocessing"
teaching: 0
exercises: 0 
questions:
- "What are some common run time configuration changes?"
- "How do I make these changess?"
objectives:
keypoints:
---

The process of going from `history` files to `timeseries` files and to convert 3D atmospheric data from the model coordinate system to selected pressure levels. We will learn how to use the CESM Postprocessing Tools

<!---
The post processing scripts are located in your ~/scripts/ directory.
You can find them using `ls *create*`.

#### Open the script in a text editor (e.g. gedit, vi, emacs)

#### Change the lines of the script relevant for your run, for example, in `atm.create_timeseries.ncl'

  run_name = "run.2"
  styr = 1
  enyr = 4
  work_dir = "/glade/scratch/cstan/"
  archive_dir = "/glade/scratch/cstan/archive/"+run_name+"/atm/hist"

#### Run the postprocessing script

~~~
ncl atm.create_timeseries.ncl
~~~
{: .language-bash}

#### The timeseries files are located in:
`/glade/scratch/cstan/processed/<case name>/`

You can take a quick look at them in `ncview`.
To do Assignment #3, you can read them in using `xarray` 

Run the post-processing for whichever component is of interest to you.

--->

~~~
$ cd postprocess/
$ cp /glade/u/home/dbailey/timeseries .
~~~
{: .language-bash}

Edit timeseries:

- edit walltime (=30 minutes for a short run) 
- edit project number
- edit CASE (=casename)
- edit CASEROOT (=where the model data sits, but do not include the casename, e.g., /glade/derecho/scratch/cstan)

Edit the settings of `env_postprocess.xml` file using `pp_config`

Set where the output should be written. You have to create this directory. If not set the output will be written in DOUT_S_ROOT.

~~~
pp_config --set TIMESERIES_OUTPUT_ROOTDIR=/glade/derecho/scratch/$USER/diagnostics-output/tseries
~~~
{: .language-bash}

Run the postprocessing:

~~~
$ qsub timeseries
~~~
{: .language-bash}



