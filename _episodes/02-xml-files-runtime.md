---
title: "Common Runtime Configuration Changes"
teaching: 0
exercises: 0 
questions:
- "What are some common run time configuration changes?"
- "How do I make these changess?"
objectives:
keypoints:
---

The `CASEROOT/env_*.xml files control how we compile and run the model. They were creatd with the `create_newcase` script.

![XMLFILES](../fig/envxmlfiles.png)

These files are changed using the `xmlchange` command, but you can look at them to see what types of change you can make. 
Let's look at `env_run.xml` in the `CASEROOT` directory for our new run. 

~~~
$ cd ~/cases/b.run2
$ more env_run.xml
~~~
{: .language-bash}

The file contains `id`s and their corresponding `values` as well as a description of the `id` and sometimes `valid_values`.
Note that these values can be changed anytime during the model run.

Scroll down (using the `spacebar`) to find the `STOP_OPTION` and `STOP_N` ids.  

~~~
    <entry id="STOP_OPTION" value="ndays">
      <type>char</type>
      <valid_values>none,never,nsteps,nstep,nseconds,nsecond,nminutes,nminute,nhours,nhour,ndays,nday,nmo
nths,nmonth,nyears,nyear,date,ifdays0,end</valid_values>
      <desc>
      Sets the run length along with STOP_N and STOP_DATE
    </desc>
    </entry>
    <entry id="STOP_N" value="5">
      <type>integer</type>
      <desc>
      Provides a numerical count for $STOP_OPTION.
    </desc>
~~~
{: .output}

The default `STOP_OPTION` is `ndays` and the default `STOP_N` is `5` which is why our first run ran for 5 days.  
Exit by typing `q`


Let's look at the file `env_batch.xml`

~~~
$ more env_batch.xml
~~~
{: .language-bash}

Scroll down (using the `spacebar`) to find the `PROJECT` id. What is it set to?
Exit by typing `q`


To find out these values without looking in the files, we use `xmlquery`
~~~
$ ./xmlquery STOP_OPTION,STOP_N,PROJECT
~~~
{: .language-bash}

~~~
Results in group case.run
	PROJECT: UGMU0032

Results in group case.st_archive
	PROJECT: UGMU0032

Results in group run_begin_stop_restart
	STOP_OPTION: ndays
	STOP_N: 5
~~~
{: .output}

To change these values, we use `xmlchange ID=VALUE`
This is great.  We don't have to remember which file they are located in!

### Some Common Configuration Changes
`STOP_N`: Run length time interval

`STOP_OPTION`: number of intervals to run during the specified wall clock time

`RESUBMIT`: number of times to resubmit a run

> ## Configure your New Case
>
> Change the STOP_OPTION and STOP_N to Configure your case to run for 48 months. 
> Do not submit your run yet!
{: .challenge}


> ## Understanding RESUBMIT
>
> This version of CESM2 (f19_g17 resolution) on Cheyenne simulates about 10 model years per wall clock day.
> Wall clock refers to actual time running on the supercomputer.   
> You can request a maximum of 12 hours of wall clock time when you submit a run.
> What this means that that if you want to run a long experiment, you have to run the number of years you can run in 12 hours, then have it resubmit
> itself where it left off.  You can do this by giving it the right combination of STOP_OPTION, STOP_N, and RESUBMIT. 
> 
> What values of `STOP_OPTION`, `STOP_N`, and `RESUBMIT` would you need to run this version of CESM for 100 years. 
>
> > ## Solution
> >
> > `STOP_OPTION`=nyears
> > `STOP_N`=5
> > `RESUBMIT`=19
> > 
> {: .solution}
>
{: .challenge}

