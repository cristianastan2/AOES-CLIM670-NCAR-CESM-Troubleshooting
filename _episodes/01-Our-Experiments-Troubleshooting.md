---
title: "Our Experiments"
questions:
- "How can I confirm if everything ran correctly?"
- "How can I troubleshoot errors?"
objectives:
keypoints:
- ""
---

We have three cases (or experiments) we are working with.  We will take a look at what happened with each one of them. 

1. b.day1.0 
: This was our first case that ran initially for 5-days.  Let's set `CONTINUE_RUN=TRUE` and ran it again for another 5-days.
This run should have completed successfully. How can we confirm this?

> ## Check your CaseStatus file
>
> Go to your case directory for this case and look at the end of the `CaseStatus` file
> ~~~
> tail CaseStatus
> ~~~
> {: .source}
{: .challenge}

Now let's check out the output from this case.  Remember, it is located in the DOUT_S_ROOT directory.
~~~
cases/b.day1.0> ./xmlquery DOUT_S_ROOT
~~~
{: .language-bash}

~~~

	DOUT_S_ROOT: /glade/scratch/cstan/archive/b.day1.0
~~~
{: .output}

We will go there and see if we now have 10 days of data.

~~~
cd /glade/scratch/cstan/b.day1.0
cd ocn/hist
~~~
{: .language-bash}

We now have two output files for the ocean: `b.day1.0.pop.h.nday1.0001-01-01.nc` and `b.day1.0.pop.h.nday1.0001-01-06.nc`.

`b.day1.0.pop.h.nday1.0001-01-01.nc` is the same file we looked at last week which has the first 5-days.  
`b.day1.0.pop.h.nday1.0001-01-06.nc` contains the next 5-days that we ran by setting `CONTINUE_RUN=TRUE`
Use ncview to look at these files.

2. Second case
: This is our case that we are running for 4-years with daily precip and standard monthly output to use for Assignment #3. Assuming the configuration and namelist changes were entered correctly, this run should have completed successfully. 

* Check your `CaseStatus` file.
* If errors, check your log file
* If no errors, check your output:  

There should be monthly and daily output for the atmosphere. Let's confirm:
~~~
cd /glade/scratch/cstan/archive/run.2/atm/hist
ls
~~~
{: .language-bash}

The `test1.cam.h0.*.nc` files contain monthly averaged data.

The `test1.cam.h1.*.nc` contain daily averaged data.

> ## What is in these files?
>
> We will look at each file using `ncdump -h` to understand what is in the files.
>
> What variables are in the h0 files?
> What variables are in the h1 files?
>
>We set this with the namelist options 
>
>`fincl2 = 'PRECC', 'PRECL'`
>
>`nhtfrq = 0, -24`
>
> How many times are in the h0 files?
>
> How many times are in the h1 files?
>
> We set this with the `mfilt = 1,1` namelist option.
>
> Remember, you can look up namelist [options](http://www.cesm.ucar.edu/models/cesm2/settings/current/)
>
> mfilt
> : Array containing the maximum number of time samples written to a history file.  The first value applies to the primary history file, the second through tenth to the auxillary history files.  Default: 1,30,30,30,30,30,30,30,30,30
>
{: .challenge}
 
We have lots of history files and we can look at each of them using `ncview`, but that is not very useful.  

We can read in all the files using Python `xarray`, but its a lot of data.

There are some useful tools for postprocessing the data to get `timeseries` files and easily take a look at some common diagnostics.  We will learn about those later in this class.

3. BRANCH case:
: This is the branch case we ran with lots of configuration changes and namelist changes.  This run produces an error with the configuration provided.

We can review how we created and setup our run by looking at the first line of the `README.case` file:

~~~
cases/branchwrong> head -n1 README.case
~~~
{: .language-bash}

~~~
2020-09-28 09:07:01: ./create_newcase --case /glade/u/home/cstan/cases/branchwrong --res f19_g17 --compset B1850 --project UGMU0042
~~~
{: .output}

You can see that I initially made a mistake in my `create_newcase` by mistyping the project number.  

We can review any changes we made to the configuration of the run by looking at `CaseStatus`

~~~
cases/branchwrong> more CaseStatus
~~~
{: .language-bash}

~~~
2020-09-28 09:13:01: xmlchange success <command> ./xmlchange RUN_TYPE=branch,RUN_REFCASE=b.day1.0,RUN_REFDATE=0001-01-05,CLM_NAMELIST_OPTS=,GET_REFCA
SE=FALSE,STOP_OPTION=nmonths,STOP_N=1,RESUBMIT=1,CCSM_CO2_PPMV=569.4  </command>
 ---------------------------------------------------
2020-09-28 09:13:20: xmlchange success <command> ./xmlchange JOB_WALLCLOCK_TIME=2:00:00  </command>
 ---------------------------------------------------
2020-09-28 09:13:24: case.setup starting
 ---------------------------------------------------
2020-09-28 09:13:25: case.setup success
 ---------------------------------------------------
2020-09-28 09:29:20: case.build starting
 ---------------------------------------------------
CESM version is cesm2.1.1-exp17
Processing externals description file : Externals.cfg
Processing externals description file : Externals_CLM.cfg
Processing externals description file : Externals_POP.cfg
Processing externals description file : Externals_CISM.cfg
Checking status of externals: clm, fates, ptclm, mosart, ww3, cime, cice, pop, cvmix, marbl, cism, source_cism, rtm, cam,
    ./cime
        clean sandbox, on cime5.6.19
    ./components/cam
        clean sandbox, on cam1/release_tags/cam_cesm2_1_rel_29/components/cam
    ./components/cice
        clean sandbox, on cice5_cesm2_1_1_20190321
    ./components/cism
        clean sandbox, on release-cesm2.0.04
    ./components/cism/source_cism
        clean sandbox, on release-cism2.1.03
    ./components/clm
        clean sandbox, on release-clm5.0.25
    ./components/clm/src/fates
        clean sandbox, on fates_s1.21.0_a7.0.0_br_rev2
    ./components/clm/tools/PTCLM
        clean sandbox, on PTCLM2_180611
    ./components/mosart
        clean sandbox, on release-cesm2.0.03
    ./components/pop
        clean sandbox, on pop2_cesm2_1_rel_n06
    ./components/pop/externals/CVMix
        clean sandbox, on v0.93-beta
    ./components/pop/externals/MARBL
        clean sandbox, on cesm2.1-n00
    ./components/rtm
        clean sandbox, on release-cesm2.0.02
    ./components/ww3
        clean sandbox, on ww3_181001
2020-09-28 09:38:13: case.build success
 ---------------------------------------------------
2020-09-28 09:38:27: case.submit starting
 ---------------------------------------------------
2020-09-28 09:38:35: case.submit error
ERROR: Command: 'qsub -q regular -l walltime=2:00:00 -A UGMU0035 -v ARGS_FOR_SCRIPT='--resubmit' .case.run' failed with error 'qsub: Invalid account, available ac
counts:
Project, Status, Active
P05010048, Normal, True
P93300190, Overspent, True
UGMU0032, Normal, True
P93300042, Overspent, True' from dir '/glade/u/home/cstan/cases/branchwrong'
 ---------------------------------------------------
2020-09-28 09:39:35: xmlchange success <command> ./xmlchange PROJECT=UGMU0032  </command>
 ---------------------------------------------------
2020-09-28 09:39:51: case.submit starting
 ---------------------------------------------------
2020-09-28 09:39:58: case.submit success case.run:4355823.chadmin1.ib0.cheyenne.ucar.edu, case.st_archive:4355824.chadmin1.ib0.cheyenne.ucar.edu
 ---------------------------------------------------
2020-09-28 11:01:58: case.run starting
 ---------------------------------------------------
2020-09-28 11:02:04: model execution starting
 ---------------------------------------------------
2020-09-28 11:02:07: model execution success
 ---------------------------------------------------
2020-09-28 11:02:07: case.run error
ERROR: RUN FAIL: Command 'mpiexec_mpt -p "%g:"  -np 576  omplace -tm open64  /glade/scratch/kpegion/branchwrong/bld/cesm.exe  >> cesm.log.$LID 2>&1 '
 failed
See log file for details: /glade/scratch/kpegion/branchwrong/run/cesm.log.4355823.chadmin1.ib0.cheyenne.ucar.edu.200928-110158
 ---------------------------------------------------
~~~
{: .output}


Another thing we did that is not documented automatically is to copy the restart files from our `b.day1.0` case to our new run directory. This was so the model has a set of restart files to start the run from.

~~~
cp /glade/scratch/cstan/archive/b.day1.0/rest/0001-01-06-00000/* /glade/scratch/cstan/branchwrong/run/ 
~~~
{: .language-bash}

### How do we figure out what went wrong?

Look at your log file and use `grep -i` to find errors.

~~~
cases/branchwrong> grep -i error /glade/scratch/cstan/branchwrong/run/cesm.log.4355823.chadmin1.ib0.cheyenne.ucar.edu.200928-110158
~~~
{: .language-bash}

~~~
16: ERROR: GETFIL: FAILED to get b.day1.0.cam.r.0001-01-05-00000.nc
4: ERROR: GETFIL: FAILED to get b.day1.0.cam.r.0001-01-05-00000.nc
29: ERROR: GETFIL: FAILED to get b.day1.0.cam.r.0001-01-05-00000.nc
33: ERROR: GETFIL: FAILED to get b.day1.0.cam.r.0001-01-05-00000.nc
34: ERROR: GETFIL: FAILED to get b.day1.0.cam.r.0001-01-05-00000.nc
3: ERROR: GETFIL: FAILED to get b.day1.0.cam.r.0001-01-05-00000.nc
22: ERROR: GETFIL: FAILED to get b.day1.0.cam.r.0001-01-05-00000.nc
18: ERROR: GETFIL: FAILED to get b.day1.0.cam.r.0001-01-05-00000.nc
...
~~~
{: .output}

*Why is the error repeated many times?*

The model runs on many processors.  Each one is reporting the error.  

*What does the error mean?*

This is telling that the model is trying to get a file called `b.day1.0.cam.r.0001-01-05-00000.nc` and it is unable to get it.

Let's go back to our configuration and think back about how we set up this experiment. What do all the configuration changes mean?
We can take a look in `env_run.xml` to confirm what each setting means.

RUN_TYPE=branch
: This is a branch run

RUN_REFCASE=b.day1.0
: Reference directory containing RUN_REFCASE data - used for hybrid or branch runs

RUN_REFDATE=0001-01-05
: Reference date for hybrid or branch runs (yyyy-mm-dd)

CLM_NAMELIST_OPTS=''
: CLM-specific namelist settings for -namelist option in the CLM
      build-namelist. CLM_NAMELIST_OPTS is normally set as a compset variable
      and in general should not be modified for supported compsets.
      It is recommended that if you want to modify this value for your experiment,
      you should use your own user-defined component sets via using create_newcase
      with a compset_file argument.
      This is an advanced flag and should only be used by expert users.

It seems this option was provided in the NCAR tutorial example, but is not necessary.

GET_REFCASE=FALSE
: Flag for automatically prestaging the refcase restart dataset. If TRUE, then the refcase data is prestaged into the executable directory

STOP_OPTION=nmonths
: Sets the run length along with STOP_N and STOP_DATE

STOP_N=1
: Provides a numerical count for $STOP_OPTION.

RESUBMIT=1
: If RESUBMIT is greater than 0, then case will automatically resubmit
Since we later set our queue time to only 2 hours, there may be a need to resubmit to complete the run.

CCSM_CO2_PPMV=569.4
: Mechanism for setting the CO2 value in ppmv for CLM if CLM_CO2_TYPE is constant or for POP if OCN_CO2_TYPE is constant.  This is the CO2 value that gets propogated to the ocean and land models.

JOB_WALLCLOCK_TIME=2:00:00
: The machine wallclock setting.
This means how long we tell it to run in the queue.  The maximum and default are 12:00:00, but we can get our run in 
more quickly if we tell it we need less time.

> ## Do you see anything in the configuration that could have led to our error?
>
> Look at the `RUN_REFDATE` and the date we used for our restart file
>
> > ## Solution
> >
> > ~~~
> > RUN_REFDATE=0001-01-06
> > cp /glade/scratch/kpegion/archive/b.day1.0/rest/0001-01-06-00000/* /glade/scratch/kpegion/test1/run/ 
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
>
> Fix it!
>
{: .challenge}

What did those namelist changes do?

[We can look them up](http://www.cesm.ucar.edu/models/cesm2/settings/current/)

In `user_nl_cam`

co2vmr=569.4e-6
: CO2 volume mixing ratio.  This is used as the time invariant surface value of CO2 if no time varying values are specified.  Default: set by build-namelist. 

ch4vmr = 1583.2e-9
: CH4 volume mixing ratio.  This is used as the time invariant surface value of CH4 if no time varying values are specified.  Default: set by build-namelist.

inithist='MONTHLY'
: Frequency that initial files will be output
This produces initial condition files monthly.

Nothing looks questionable there.

Resubmit your case!


 
{% include links.md %}

