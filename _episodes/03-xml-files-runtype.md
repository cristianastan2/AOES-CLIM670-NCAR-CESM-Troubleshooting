---
title: "Setting the RUN TYPE"
teaching: 0
exercises: 0 
questions:
- "What are CESM RUN Types and how do I set them?"
objectives:
keypoints:
---

CESM has three `types` of runs that define how the model starts or is initialized.  They are set by the `RUN_TYPE` variable in the `env_run.xml` files.

1. `STARTUP`: All model components are initialized from basic default initial conditions.  
2. `HYRBRID`: Model components are started from a user-specific CESM simulation., but I want to make some modifications to the run itself. 
3. `BRANCH': Model components are started from exact restart files and mimic exactly what the model would have done if it had been allowed to continue from where it was started.

When to use `HYBRID` vs. `BRANCH`

Use `HYBRID` when you do not need bit-for-bit reproducibility.  For example, I want to start a simulation from a date in a previous one where I make a change to some part of the model.  Then I want to compare my new run with the previous one. This would be a `HYBRID` simulation.

Use `BRANCH` when you want the model to reproduce exactly what it would have done if it have never been stopped, but you need to change it to have higher frequency output part way through the run.  

If you use `HYBRID` or `BRANCH` you also specify `RUN_REFCASE` to tell it what case you are starting from and `RUN_REFDATE` to specify the date stamp of the reference case you are starting with

> ## What is the `RUN_TYPE` for our new experiment?
>
> Use `xmlquery` to find out the `RUN_TYPE` for our new experiment
>
{: .challenge}

