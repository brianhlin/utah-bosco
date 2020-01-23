# Hosted-CE Config Overrides for Univ of Utah CHPC

## What is this?
The [OSG HostedCE](https://opensciencegrid.org/docs/compute-element/htcondor-ce-overview/) is an entrypoint for OSG jobs entering local resources. The CE will accept jobs from the OSG factory and route them appropriately to local resources. In this case, SLURM clusters running at the CHPC.

OSG HostedCEs are deployed locally through [SLATE](https://slateci.io). There is a unique instance of the CE running for each backing cluster at the CHPC (Lonepeak, Kingspeak, and Notchpeak currently). These instances can be managed, stopped, started, and updated using the [SLATE Client](https://slateci.io/docs/tools/index.html) and these overrides.

[BOSCO](https://osg-bosco.github.io/docs/) is a job submission manager responsible for submitting jobs from the HostedCE to the remote cluster. This repository contains configuration overrides for the bosco component. These overrides allow you to set non-standard values for configs such as the slurm binary path, and slurm submission parameters. A component of the CE clones these overrides into a directory on the remote cluster used by BOSCO for job submission.

Additionally, the configuration used by SLATE is stored alongside the bosco overrides. This repository acts a central home and record for all configurations needed by each OSG HostedCE supporting the University of Utah's CHPC. Configuration changes should first be recorded here, then applied to the running instance(s).

## Obtaining and Using the SLATE Client

In order to interact with the local CE instances you must obtain the SLATE Client and valid SLATE group membership for the group `slate-dev`. This will allow you to manage these instance using SLATE.

First hop over to https://portal.slateci.io/cli and sign in using your institutional credentials. This will create your SLATE account. From here you can copy and paste the script into your local environment to add your SLATE user credentials (A SLATE Token and API Endpoint). These are simple text files that the SLATE Client expects to find under `$HOME/.slate/`.

Next download the client following the instructions on that same page. Once this is done you should be able to see some clusters by running `slate cluster list`.

Then hop over to https://portal.slateci.io/public_groups/slate-dev and request to join the group. Once this request is fulfilled you will have full access to manage the OSG HostedCE instances supporting CHPC.

Full reference on the SLATE Client CLI can be found at https://slateci.io/docs/tools/index.html

You can view the running instances with the command `slate instance list --cluster uutah-prod`

```
Name                      Group     ID
condor-login              slate-dev instance_sVzib4wBWQs
osg-frontier-squid-global slate-dev instance_OnT-sClDSiU
osg-hosted-ce-kingspeak   slate-dev instance_4shGbDGoB6w
osg-hosted-ce-notchpeak   slate-dev instance_Vyir_kuBf3Q
```

We are specifically interested in only instances of HostedCE so, `slate instance list --cluster uutah-prod |grep osg-hosted-ce`

Each instance will have a helpful label to determine which backing cluster it is attached to.

```
osg-hosted-ce-kingspeak   slate-dev instance_4shGbDGoB6w
osg-hosted-ce-notchpeak   slate-dev instance_Vyir_kuBf3Q
```

The last column of this entry gives us the SLATE Instance ID, which we need to manage the instance. 

An instance can be paused by running `slate instane scale --replicas 0 <SLATE INSTANCE ID>`

The instance can be unpaused by running `slate instance scale --replicas 1 <SLATE INSTANCE ID>`

You can do a simple restart of the instance by running `slate instance restart <SLATE INSTANCE ID>`

You can delete the instance by running `slate instance delete <SLATE INSTANCE ID>`

To view instance specific logs run `slate instance logs <SLATE INSTANCE ID> --max-lines 0`

*the --max-lines=0 flag prevents truncated output*

## How to Configure

TODO
