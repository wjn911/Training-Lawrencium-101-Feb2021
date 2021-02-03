% Lawrencium 101 
% February 11, 2021
% Wei Feinstein

# Outline
- Orientation of Lawrencium supercluster
- Access/login to clusters
- Access software on the system
- Job submission and monitoring
- Open Ondemand with Jupyter notebooks
- Data transfer to/from clusters
- Remote visualization  

# Lawrencium Cluster Overview
- A computing service provided by the IT Division to support researchers in all disciplines at the Lab
- Help the scientists solve various problems  that positively impact the world.  
- Lawrencium is a LBNL Condo Cluster Computing program
  - Significant investment from LBNL
  - Individual PIs purchase nodes and storage
  - Computational cycles are shared among all lawrencium users

# System Capabilities
IT Division & Condo Contributions
1058 Compute nodes
30,192 CPUs
150 GPUs
712 User Accounts
317 Groups 

Standalone Clusters
UC Berkeley  
Advanced Light Source   
Nuclear Science Division   
Applied Nuclear Physics
Biological Systems Engineer Division

# Conceptual diagram of Lawrencium
<left><img src="figures/lrc.png" width="70%"></left>

[Detailed informaton of Lawrencium](https://sites.google.com/a/lbl.gov/high-performance-computing-services-group/lbnl-supercluster/lawrencium)

# Getting access to Lawrencium

### Three types of Project Accounts
- Primary Investigator (PI) Computing Allowance (PCA) account: free 300K SUs per year (pc_xxx)
- Condo account: PIs can purchage compute nodes to be added to the general pool, in exchage for their own priority access and share the Lawrencium infrastructure (lr_xxx)
- Recharge account: pay as you go with minimal recharge rate ~ $0.01/SU (ac_xxx)
- PIs can add researchers/students working with them to get user accounts with access to the PCA/condo/recharge resources available to them
  - User account request
- User agreement consent
[https://sites.google.com/a/lbl.gov/hpc/getting-an-account]([https://sites.google.com/a/lbl.gov/hpc/getting-an-account)

# Login
- Linux terminal (command-line) session. 
- Mac terminal (see Applications -> Utilities -> Terminal). 
- Windows [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).
- One-time passwords (OPT): set up your smartphone or tablet with Google Authenticator 
- Login:
```
ssh $USER@lrc-login.lbl.gov
```
- Followed by password prompt 
```
$ password:

```
- Enter your PIN followed by the one-time password from which your Google Authenticator app generates on your phone / tablet.
** DO NOT run jobs on login nodes!! **

# Data Transfer
- scp, rsync on lrc-xfer.lbl.gov
```
# On local machine transfer to Lawrencium
scp file-xxx $USER@lrc-xfer.lbl.gov:/global/home/users/$USER
scp -r dir-xxx $USER@lrc-xfer.lbl.gov:/global/scratch/$USER

# On Local machine transfer from Lawrencium
scp $USER@lrc-xfer.lbl.gov:/global/scratch/$USER/file-xxx ~/Desktop

# Transfer to another institute
ssh $USER@lrc-xfer.lbl.gov
scp -r file-on-lawrencium $USER@other-institute:/destination/path/$USER
```
- On Window
  - [WinSCP](https://winscp.net/eng/index.php)
  - [FileZella](https://filezilla-project.org/): multi-platform program via SFTP

# Globus Data Transfer
- Transfer data faster and unattended between endpoints, see [instructions](https://sites.google.com/a/lbl.gov/high-performance-computing-services-group/getting-started/data-transfer)
- Possible endpoints include: lbnl#lrc, your laptop/desktop, NERSC, among others.
- Transfer data to/from your laptop (endpoint setup):
 - Globus Connect Personal [set up](https://www.globus.org/globus-connect-personal)
 - Your machine established as an endpoint
 - Globus Connect Pesonal actively running on your machine. 

<left><img src="figures/globus.jpg" width="70%"></left>
- 

# Softwre Module Farm 

### Module commands
- *module purge*: clear user’s work environment
- *module avail*: check available software packages
- *module load xxx*: load a package
- *module list*: check currently loaded software 
- Users may install their own software

[https://sites.google.com/a/lbl.gov/high-performance-computing-services-group/getting-started/sl6-module-farm-guide](https://sites.google.com/a/lbl.gov/high-performance-computing-services-group/getting-started/sl6-module-farm-guide)

# SLURM: Resource Manager & Job Scheduler
 
### Jub Submission

- Get help with the complete command options
 ``` 
sbatch --help
 ```
- sbatch: submit a job to the batch queue system
```
sbatch myjob.sh
```
- srun: request an interactive node(s) and login automatically
```
srun -A ac_xxx -p lr5 -q lr_normal -t 1:0:0 --pty bash
```
- salloc : request an interactive node(s)
```
salloc –A pc_xxx –p lr6 –q lr_debug –t 0:30:0
```
# Job Monitoring

- sinfo: check status of partitions and nodes (idle, allocated, drain, down) 
 ```
 sinfo –r –p lr6
 ```
- squeue: check jobs in the batch queuing system (R or PD)
```
squeue –u $USER
```
- sacct: check job information or history
```
sacct -X -o 'jobid,user,partition,nodelist,stat'
```
- scancel : cancel a job
```
scancel jobID
```
- Check slurm association, such as qos, account, partition
```
sacctmgr show association user=wfeinstein -p
perceus-00|ac_test|wfeinstein|lr6|1||||||||||||lr_debug,lr_lowprio,lr_normal|||
perceus-00|ac_test|wfeinstein|lr5|1||||||||||||lr_debug,lr_lowprio,lr_normal|||
perceus-00|pc_test|wfeinstein|lr4|1||||||||||||lr_debug,lr_lowprio,lr_normal|||
perceus-00|lr_test|wfeinstein|lr3|1||||||||||||lr_debug,lr_lowprio,lr_normal,te
perceus-00|scs|wfeinstein|es1|1||||||||||||es_debug,es_lowprio,es_normal|||
```
[https://sites.google.com/a/lbl.gov/high-performance-computing-services-group/scheduler/slurm-usage-instructions](https://sites.google.com/a/lbl.gov/high-performance-computing-services-group/scheduler/slurm-usage-instructions)

# Services (1)

- Designate data transfer node **lrc-xfer.lbl.gov**
    - scp -r /your/source/file  $USER@lrc-xder.lbl.gov:/cluster/path
    - rsync -avzh /your/source/file $USER @lrc-xfer.lbl.gov:/cluster/path
- Globus Online provide secured unified interface for data transfer
    - endpoint lbn#lrc, Globus Connect, AWS S3 connector
- Visualization and remote desktop node **viz.lbl.gov**
    - Detailed information [click here](https://sites.google.com/a/lbl.gov/high-performance-computing-services-group/getting-started/remote-desktop)

[LRC Jupyterhub](https://lrc-jupyter.lbl.gov)
<left><img src="figures/jupyter.png" width="60%"></left>


# Services (3): Cloud Computing

- LBNL has a master payer program for cloud services on Amazon Web Services (AWS) and Google Cloud Platform (GCP).  
* No charge to have an account in the program
* Charges only for actual usage of cloud services like storage or compute
* De-enrollment only changes the billing setup in your account, and your account will continue to be active.
* Complete control of your own AWS or GCP dashboard, your data, tools, and services.

# AWS & GCP Services 

- AWS and GCP make discounts available to LBNL users.

| Type | AWS | GCP |  |
| --- | --- | --- | --- |
| Overall | 7% | 13% | |
| Data Egress | 15% | 25% | 

- AWS, [restrictions apply](https://aws.amazon.com/blogs/publicsector/aws-offers-data-egress-discount-to-researchers/) 
- Over 125 people at LBNL with cloud accounts on AWS and GCP.  
- Mostly use virtual machines and storage, containers, ML, AI, and data visualization 
- To set up a cloud account on either AWS or GCP, send email to [scienceit@lbl.gov](mailto:scienceit@lbl.gov)

# Virtual Machine Services
<left><img src="figures/vm.png" width="40%"></left>

- Submit a slurm job 

# Job Submission Example
```
#!/bin/bash -l
#SBATCH --job-name=container-test		 
#SBATCH --partition=lr5			 
#SBATCH --account=ac_xxx		 
#SBATCH --qos=lr_normal			
#SBATCH --nodes=1			
#SBATCH --time=1-2:0:0			

```

# Exercise

- 1) singularity pull hello-world.sif shub://singularityhub/hello-world
- 2) generate hello-world.def or generate a sandbox to start with
- 3) add /data directory inside the container
- 4) build a new image hello-world-new.sif
- 5) bind /home/$USER on host to /data inside container
- 6) ls /data inside the new container  

# Getting help
- Virtual Office Hours:
    - Time: 10:30am - noon (Wednesdays) 
    - Request [online](https://docs.google.com/forms/d/e/1FAIpQLScBbNcr0CbhWs8oyrQ0pKLmLObQMFmYseHtrvyLfOAoIInyVA/viewform)
- Sending us tickets at hpcshelp@lbl.gov
- More information, documents, tips of how to use LBNL Supercluster [http://scs.lbl.gov/](http://scs.lbl.gov)
- DLab consulting: [https://dlab.berkeley.edu/consulting](https://dlab.berkeley.edu/consulting)

Please fill out [Training Survey](https://docs.google.com/forms/d/e/1FAIpQLSdrmW-7gZ8FankQwEceY6r_uXPmLHAuXFjDDfwu-86A1a0llg/viewform) to get your comments and help us improve.  

