---
title: "Namelist changes"
teaching: 0
exercises: 0 
questions:
- "How do I make namelist configuration changes"
objectives:
keypoints:
---
Not all changes are made in the `env_*.xml` files. Some are make in `CASEROOT/user_nl_*`.  These changes are specific to each component.  

To make a namelist change, add `namelist_var=new_namelist_var` to `user_nl_*` for the specific component you want to change.

When you add changes to the `user_nl_*` files, model uses this to create the namelists it will use when it runs.  We can see what this means:

~~~
$ cd ~/cases/b.run2
$ ./preview_namelists
$ ./xmlquery RUNDIR
$ cd /glade/scratch/kpegion/b.day1.0/run
$ more atm_in
~~~
{: .language-bash}

How do I know what namelist options there are to change for each component?
http://www.cesm.ucar.edu/models/cesm2/settings/current/


> ## Configure your new experiment
>
> Change the output frequency of your experiment to produce daily precipitation output
> along with default monthly output for the default set of variables.
>
> In `user_nl_atm`, add the following lines:
>
> `fincl2='PRECC','PRECL'` # higher frequency output for PRECC and PRECL (convective and large scale precipitation)
>
> `nhtfrq=0,-24` # h0 files will be monthly with default variables; h1 files will be daily with fincl2 variables
>
> `mfilt = 1,1` # h0 files will be once monthly, h1 files will be once daily
>
> Check that everything is set properly wiht `./preview_namelists`
{: .challenge}

