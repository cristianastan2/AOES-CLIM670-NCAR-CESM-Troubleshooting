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


The post processing scripts are located in your ~/scripts/ directory.
You can find them using ls *create*.

#### Open the script in a text editor (e.g. gedit, vi, emacs)

#### Change the lines of the script relevant for your run, for example, in `atm.create_timeseries.ncl'

  run_name = "test1"
  styr = 1
  enyr = 4
  work_dir = "/glade/scratch/kpegion/"
  archive_dir = "/glade/scratch/kpegion/archive/"+run_name+"/atm/hist"

#### Run the postprocessing script

~~~
ncl atm.create_timeseries.ncl
~~~
{: .language-bash}

#### The timeseries files are located in:
`/glade/scratch/kpegion/processed/<case name>/`

You can take a quick look at them in `ncview`.
To do Assignment #2, you can read them in using `xarray` 

Run the post-processing for whichever component is of interest to you.

