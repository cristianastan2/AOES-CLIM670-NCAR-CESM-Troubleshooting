---
title: "Model Diagnostics Packages"
teaching: 0
exercises: 0 
questions:
- "How do I run the model diagnostics packages?"
objectives:
keypoints:
---

### Some requirements for the diagnostics packages

Each component diagnostics package has minimum requirements for how much data must be available to run them:
* Ocean: 12months
* Atmosphere, Land: 14 months
* Ice: 24 months

Some other requirements:
* Only complete years can be analyzed by the packages.
* There must be an additional Dec before the 1st analyzed year or an additional Jan and Feb after the last analyzed year.
* If you have 4 complete years, you must set the first year to analyze to 1 and the last year to 3 or the first year to 2 and the last year to 4.

### Run a Diagnostics Package 

Select and run a diagnostics package of interest to you:


> ## Atmosphere Diagnostics Package
> 
> Edit the settings for the `env_diags_atm.xml` file using `pp_config`
> 
> ~~~
> ./pp_config --set ATMDIAG_OUTPUT_ROOT_PATH=/glade/scratch/kpegion/diagnostics-output/atm
> ./pp_config --set ATMDIAG_test_first_yr=1
> ./pp_config --set ATMDIAG_test_nyrs=3
> ~~~
> {: .language-bash}
>
> 
> Run the monthly climatologies
> ~~~
> qsub atm_averages -A UGMU0032
> ~~~
> {: .language-bash}
>
> You can monitor your job status using `qstat -u`
> Check the log file in logs to make sure everything ran ok. 
> This should run relatively quickly (only a few minutes)
>
> Once the averages are done, you can submit the diagnostics script:
>
> ~~~
> qsub atm_diagnostics -A UGMU0032
> ~~~
> {: .language-bash}
>
> This will also run relatively quickly (few minutes).  
> Once it is done, you can go to the location of the diagnostics and look at the output via a webpage:
>
> ~~~
> cd /glade/scratch/kpegion/diagnostics-output/atm/diag/test2-obs.1-3
> firefox index.html &
> ~~~
> {: .language-bash}
>
> It may be slow for your web browser window to launch and display
> depending on your bandwidth.
>
> For more information on the Atmosphere (AMWG) Diagnostics Package: http://www.cesm.ucar.edu/working_groups/Atmosphere/amwg-diagnostics-package/
>
{: .callout}


> ## Land Diagnostics Package
>
> Edit the settings for the `env_diags_land.xml` file using `pp_config`
>
> ~~~
> ./pp_config --set LNDDIAG_OUTPUT_ROOT_PATH=/glade/scratch/kpegion/diagnostics-output/lnd
> ./pp_config --set LNDDIAG_clim_first_yr_1=1
> ./pp_config --set LNDDIAG_clim_num_yrs_1=3
> ./pp_config --set LNDDIAG_trends_first_yr_1=1
> ./pp_config --set LNDDIAG_trends_num_yrs_1=3
> ~~~
> {: .language-bash}
>
>
> Run the monthly climatologies
> ~~~
> qsub lnd_averages -A UGMU0032
> ~~~
> {: .language-bash}
>
> You can monitor your job status using `qstat -u`
> Check the log file in logs to make sure everything ran ok. 
> This should run relatively quickly (only a few minutes)
>
> Once the averages are done, you can submit the diagnostics script:
>
> ~~~
> qsub lnd_diagnostics -A UGMU0032
> ~~~
> {: .language-bash}
>
> This will also run relatively quickly (few minutes).
> Once it is done, you can go to the location of the diagnostics and look at the output via a webpage:
>
> ~~~
> cd /glade/scratch/kpegion/diagnostics-output/lnd/diag/test2-obs.1_3
> firefox setsIndex.html &
> ~~~
> {: .language-bash}
>
> It may be slow for your web browser window to launch and display
> depending on your bandwidth.
>
> For more information the Land (LMWG) Diagnostics Package: http://www.cesm.ucar.edu/models/cesm1.2/clm/clm_diagpackage.html
>
{: .callout}

> ## Ocean Diagnostics Package
>
> Edit the settings for the `env_diags_ocn.xml` file using `pp_config`
>
> ~~~
> ./pp_config --set OCNDIAG_YEAR0=1
> ./pp_config --set OCNDIAG_YEAR1=3
> ./pp_config --set OCNDIAG_TSERIES_YEAR0=1
> ./pp_config --set OCNDIAG_TSERIES_YEAR1=3
> ./pp_config --set OCNDIAG_TAVGDIR=/glade/scratch/kpegion/diagnostics-output/ocn/climo/tavg.\$OCNDIAG_YEAR0.\$OCNDIAG_YEAR1
> ./pp_config --set OCNDIAG_WORKDIR=/glade/scratch/kpegion/diagnostics-output/ocn/diag/test2.\$OCNDIAG_YEAR0.\$OCNDIAG_YEAR1
> ~~~
> {: .language-bash}
>
>
> Run the monthly climatologies
> ~~~
> qsub ocn_averages -A UGMU0032
> ~~~
> {: .language-bash}
>
> You can monitor your job status using `qstat -u`
> Check the log file in logs to make sure everything ran ok.
> This should run relatively quickly (only a few minutes)
>
> Once the averages are done, you can submit the diagnostics script:
>
> ~~~
> qsub ocn_diagnostics -A UGMU0032
> ~~~
> {: .language-bash}
>
> This will also run relatively quickly (few minutes).
> Once it is done, you can go to the location of the diagnostics and look at the output via a webpage:
>
> ~~~
> cd /glade/scratch/kpegion/diagnostics-output/ocn/diag/test2.1_3
> firefox index.html &
> ~~~
> {: .language-bash}
>
> It may be slow for your web browser window to launch and display
> depending on your bandwidth.
>
{: .callout}

> ## Ice Diagnostics Package
>
> Edit the settings for the `env_diags_ic.xml` file using `pp_config`
>
> ~~~
> ./pp_config --set ICEDIAG_BEGYR_CONT=1
> ./pp_config --set ICEDIAG_ENDYR_CONT=3
> ./pp_config --set ICEDIAG_YRS_TO_AVG=3
> ./pp_config --set ICEDIAG_PATH_CLIMO_CONT=/glade/scratch/kpegion/diagnostics-output/ice/climo/\$ICEDIAG_CASE_TO_CONT/
> ./pp_config --set ICEDIAG_DIAG_ROOT=/glade/scratch/kpegion/diagnostics-output/ice/diag/\$ICEDIAG_CASE_TO_CONT/
> ~~~
> {: .language-bash}
>
>
> Run the monthly climatologies
> ~~~
> qsub ice_averages -A UGMU0032
> ~~~
> {: .language-bash}
>
> You can monitor your job status using `qstat -u`
> Check the log file in logs to make sure everything ran ok.
> This should run relatively quickly (only a few minutes)
>
> Once the averages are done, you can submit the diagnostics script:
>
> ~~~
> qsub ice_diagnostics -A UGMU0032
> ~~~
> {: .language-bash}
>
> This will also run relatively quickly (few minutes).
> Once it is done, you can go to the location of the diagnostics and look at the output via a webpage:
>
> ~~~
> cd /glade/scratch/kpegion/diagnostics-output/ice/diag/test2.1_3
> firefox index.html &
> ~~~
> {: .language-bash}
>
> It may be slow for your web browser window to launch and display
> depending on your bandwidth.
>
>
{: .callout}

### Try the CVDP

Our runs are not long enough to run the CVDP, but you can test it on an existing long simulation, the CESM Large Ensemble.

#### On Cheyenne, we need to get an analysis node to Casper

~~~
execdav --account=UGMU0032
cd ~/scripts/CVDP
~~~
{: .language-bash}

#### Open the file `namelist` using your preferred text editor

The format of the file is:
`Run Name | Path to all data for a simulatoin | Analysis start year | Analysis end year`

#### Modify the rows so that the analysis start and end years are 1979 and 2015.  

Open up the file `driver.ncl` using your preferred text editor

*On line 7: replace `user` with your username*
*On line 19: change `False` to  `True` to output calculations in nceCDF*

#### Run the CVDP by typing

~~~
ncl driver.ncl
~~~
{: .language-bash}

It will take ~20 minutes.  Once it is complete, go to the output directory and open a firefox window

~~~
cd /glade/scatch/kpegion/CVDP
firefox index.html&
~~~
{: .language-bash}
