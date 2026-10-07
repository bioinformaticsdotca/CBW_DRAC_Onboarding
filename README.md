# CBW Computing Onboarding: Linux, Slurm, and DRAC HPC

Welcome to the **CBW Computing Onboarding Workshop**.

In this workshop, we will use a **training High-Performance Computing (HPC) cluster** to learn the basic computing skills required for CBW courses.

The cluster provides an environment similar to research computing systems operated by the Digital Research Alliance of Canada (DRAC). You will learn how to:

- connect to an HPC cluster;
- work with files using Linux;
- transfer data between your computer and the cluster;
- use scientific software;
- submit and monitor computational jobs;
- request interactive computing resources;
- use JupyterHub;
- run a small bioinformatics analysis.

---

# Workshop Duration

**4 hours**

Most of the workshop will be hands-on.

---

# Learning Objectives

By the end of this workshop, you should be able to:

- understand the basic structure of an HPC cluster;
- register an account and connect to the training cluster using SSH;
- navigate directories and work with files using Linux commands;
- transfer files between your computer and the cluster;
- find and load scientific software using environment modules;
- understand basic CPU, memory, and runtime requirements;
- submit jobs using Slurm;
- monitor and cancel jobs;
- use interactive computing through `salloc`;
- launch interactive analysis environments through JupyterHub;
- run a simple bioinformatics workflow on the training cluster.

---

# Workshop Schedule

| Time | Topic |
|---|---|
| 11:00–11:25 | 1. Introduction to HPC Infrastructure |
| 11:25–11:45 | 2. Register and Log In |
| 11:45–12:15 | 3. Browse Your Directory |
| 12:15–12:40 | 4. Transfer Files In and Out |
| 12:40–12:50 | Break |
| 12:50–13:15 | 5. Software Environment |
| 13:15–14:00 | 6. Submit and Monitor Jobs |
| 14:00–14:10 | Break |
| 14:10–14:40 | 7. Interactive Computing |
| 14:40–15:00 | 8. Bioinformatics Mini-Challenge |

---

# 1. Introduction to HPC Infrastructure

High-Performance Computing systems are designed to allow many researchers to share large computing resources.

A typical HPC cluster contains several different components:

```text
                       HPC Cluster

Your computer
     |
     | SSH
     v
+-------------+
| Login Node  |
+-------------+
      |
      | Submit jobs
      v
    Slurm
      |
      +-------------------------+
      |            |            |
      v            v            v
+-----------+ +-----------+ +-----------+
| Compute   | | Compute   | | Compute   |
| Node 1    | | Node 2    | | Node 3    |
+-----------+ +-----------+ +-----------+
      \            |            /
       \           |           /
        +---------------------+
        |   Shared Storage    |
        +---------------------+
```

## Login Node

When you first connect to the cluster, you normally arrive at a **login node**.

The login node is used for tasks such as:

- navigating directories;
- viewing and editing files;
- transferring files;
- loading software environments;
- preparing scripts;
- submitting jobs;
- monitoring jobs.

The login node is shared by many users.

> **Do not run computationally intensive analyses directly on the login node.**

---

## Compute Nodes

Compute nodes provide the resources used to run analyses.

They contain resources such as:

- CPUs;
- RAM;
- GPUs;
- temporary local storage.

You normally access compute nodes through the cluster scheduler.

---

# Slurm Scheduler

The training cluster uses **Slurm** to manage computational jobs.

Instead of choosing a compute node yourself, you tell Slurm what resources your analysis needs.

For example:

```text
CPUs:     4
Memory:   8 GB
Time:     1 hour
```

Slurm then finds an appropriate compute node and runs your job.

```text
User
 |
 | submit job
 v
Slurm
 |
 | find available resources
 v
Compute Node
 |
 v
Run analysis
```

---

# Shared Computing

An HPC cluster is a **shared system**.

Many users may be:

- submitting jobs;
- accessing data;
- loading software;
- using CPUs;
- using memory;
- using GPUs;

at the same time.

Therefore, good HPC practice includes:

- requesting only the resources you need;
- using compute nodes for computational work;
- keeping files organized;
- monitoring your jobs;
- avoiding unnecessary use of shared resources.

---

# 2. Register and Log In

Before using the training cluster, you need to create an account.

## Step 1 — Register

Open the workshop registration page:

**[CBW Training Cluster Registration](https://mokey.cbw.c3.ca)**

Create your account using:

- a **username**;
- a **password**;
- your **email address**.

Remember the username and password you create. You will use them throughout the workshop.

---

# Step 2 — Open a Terminal

## macOS

Open:

```text
Applications → Utilities → Terminal
```

## Linux

Open your preferred terminal application.

## Windows

You can use:

- Windows Terminal;
- PowerShell;
- WSL.

For basic SSH access, Windows Terminal or PowerShell is sufficient.

---

# Step 3 — Connect to the Cluster

Use:

```bash
ssh username@cbw.c3.ca
```

Replace `username` with the username you registered.

For example:

```bash
ssh jerry@cbw.c3.ca
```

You will be asked for your password.

---

## First Connection

The first time you connect, SSH may display a message similar to:

```text
The authenticity of host 'cbw.c3.ca' can't be established.

Are you sure you want to continue connecting?
```

Type:

```text
yes
```

and press **Enter**.

Then enter your password.

---

## Check Where You Are

After logging in, run:

```bash
hostname
```

You should see the hostname of the cluster login node.

You can also check your username:

```bash
whoami
```

---

## Log Out

To disconnect from the cluster:

```bash
exit
```

---

# 3. Browse Your Directory

Once you log in, you are working in a Linux environment.

You do not need to know every Linux command. A small number of commands will cover most tasks in this workshop.

---

# Where Am I?

Run:

```bash
pwd
```

`pwd` means:

```text
print working directory
```

Example:

```text
/home/jsmith
```

---

# What Files Are Here?

```bash
ls
```

For more information:

```bash
ls -lh
```

The options mean:

```text
-l    long format
-h    human-readable file sizes
```

---

# Show Hidden Files

```bash
ls -la
```

Files beginning with `.` are normally hidden.

---

# Change Directory

```bash
cd directory_name
```

For example:

```bash
cd data
```

---

## Go Up One Directory

```bash
cd ..
```

---

## Return to Your Home Directory

```bash
cd ~
```

or simply:

```bash
cd
```

---

# Create a Directory

```bash
mkdir results
```

Check it:

```bash
ls
```

---

# Copy a File

```bash
cp source.txt copy.txt
```

---

# Move or Rename a File

```bash
mv old.txt new.txt
```

---

# Remove a File

```bash
rm file.txt
```

Be careful with `rm`.

> Files deleted using `rm` normally do not go to a recycle bin.

---

# Browse File Contents

Show the first few lines:

```bash
head file.txt
```

Show the last few lines:

```bash
tail file.txt
```

Browse interactively:

```bash
less file.txt
```

Press:

```text
q
```

to exit `less`.

---

# Count Lines

```bash
wc -l file.txt
```

---

# Useful Bioinformatics Examples

## Count FASTA sequences

A FASTA sequence normally begins with a header line starting with `>`.

```bash
grep "^>" sequences.fa
```

Count them:

```bash
grep "^>" sequences.fa | wc -l
```

---

## Look Inside a Compressed FASTQ File

```bash
zcat reads.fastq.gz | head
```

---

# Pipes

The `|` symbol is called a **pipe**.

It sends the output of one command into another command.

For example:

```bash
grep "^>" sequences.fa | wc -l
```

means:

```text
Find FASTA headers
        |
        v
Count the number of lines
```

---

# 4. Transfer Files In and Out of the Cluster

Your data may begin on your laptop and need to be transferred to the cluster.

You may also need to download results from the cluster after your analysis finishes.

We will introduce two approaches:

1. `rsync`
2. FileZilla

---

# Method 1 — `rsync`

`rsync` is a command-line tool for transferring and synchronizing files.

It is particularly useful for:

- large files;
- directories;
- repeated transfers;
- restarting interrupted transfers.

---

## Important

Run `rsync` from **your local computer**, not after logging into the cluster, unless you specifically intend to transfer between two remote systems.

---

# Upload a File

From your local computer:

```bash
rsync -av myfile.txt username@cbw.c3.ca:~/
```

This copies:

```text
myfile.txt
```

to your home directory on the cluster.

---

# Upload a Directory

```bash
rsync -av mydata/ username@cbw.c3.ca:~/mydata/
```

---

# Show Transfer Progress

For larger transfers:

```bash
rsync -av --progress mydata/ username@cbw.c3.ca:~/mydata/
```

---

# Download a File

To copy a file from the cluster to your current local directory:

```bash
rsync -av username@cbw.c3.ca:~/results.txt .
```

The final:

```text
.
```

means:

> the current directory on your local computer.

---

# Download a Directory

```bash
rsync -av username@cbw.c3.ca:~/results/ .
```

---

# Why `rsync` Is Useful

Suppose a large transfer is interrupted.

You can normally run the same command again:

```bash
rsync -av --progress mydata/ username@cbw.c3.ca:~/mydata/
```

`rsync` compares the source and destination and transfers what is needed.

---

# Method 2 — FileZilla

FileZilla provides a graphical interface for transferring files.

This can be convenient if you prefer drag-and-drop file management.

Use an **SFTP** connection.

Typical settings are:

```text
Protocol: SFTP
Host: cbw.c3.ca
Username: your registered username
Password: your registered password
Port: 22
```

After connecting, FileZilla normally displays:

```text
Your computer              Training cluster
----------------           ----------------
Local files                Remote files
Local directories    <->   Remote directories
```

You can drag files between the two panels.

---

# Which Transfer Method Should I Use?

| Method | Best For |
|---|---|
| FileZilla | beginners, small transfers, drag-and-drop |
| `rsync` | large datasets, directories, repeated transfers |
| `rsync` | restarting incomplete transfers |
| `rsync` | scripting and automation |

For bioinformatics projects involving many gigabytes of data, learning `rsync` is highly recommended.

---

# 5. Software Environment

HPC systems often provide many scientific applications centrally.

Instead of every user installing a separate copy, software is frequently provided using **environment modules**.

---

# Search for Software

For example:

```bash
module spider samtools
```

You can also use:

```bash
module avail
```

---

# Load Software

```bash
module load samtools
```

---

# Check That It Works

```bash
samtools --version
```

---

# Find the Program

```bash
which samtools
```

Example:

```text
/cvmfs/.../bin/samtools
```

---

# See Loaded Modules

```bash
module list
```

---

# Remove All Loaded Modules

```bash
module purge
```

---

# Software Versions Matter

For reproducible research, you should record the software version used in an analysis.

For example:

```bash
samtools --version
```

Your analysis documentation should ideally record:

```text
software
version
input data
parameters
job script
```

---

# Why Not Install Everything Yourself?

Using centrally managed software can provide several advantages:

- software is already compiled;
- dependencies are managed;
- different versions can coexist;
- installations are shared;
- environments are easier to reproduce.

For many common bioinformatics applications, check whether a module already exists before installing your own copy.

---

# 6. Submit and Monitor Jobs

Computational analysis should normally run on compute nodes.

The standard way to do this is to submit a **Slurm batch job**.

---

# A Simple Slurm Job

Create a file:

```text
hello_slurm.sh
```

with the following contents:

```bash
#!/bin/bash

#SBATCH --job-name=cbw_test
#SBATCH --time=00:05:00
#SBATCH --cpus-per-task=1
#SBATCH --mem=1G
#SBATCH --output=cbw_test_%j.out

hostname
date

echo "Hello from CBW"

sleep 10

date
```

---

# Understanding the Script

The lines beginning with:

```text
#SBATCH
```

tell Slurm what resources the job needs.

For example:

```bash
#SBATCH --time=00:05:00
```

requests a maximum runtime of 5 minutes.

```bash
#SBATCH --cpus-per-task=1
```

requests one CPU core.

```bash
#SBATCH --mem=1G
```

requests 1 GB of RAM.

---

# Submit the Job

```bash
sbatch hello_slurm.sh
```

You should see something similar to:

```text
Submitted batch job 12345
```

`12345` is the job ID.

Every submitted job receives a unique job ID.

---

# Monitor Your Jobs

```bash
squeue -u $USER
```

Example:

```text
JOBID   NAME       USER     ST   TIME   NODELIST
12345   cbw_test   jsmith   R    0:05   node01
```

---

# Common Job States

| State | Meaning |
|---|---|
| `PD` | Pending |
| `R` | Running |
| `CG` | Completing |

A typical job lifecycle is:

```text
sbatch
   |
   v
PENDING
   |
   v
RUNNING
   |
   v
COMPLETED
```

---

# Why Is My Job Pending?

A pending job is not necessarily a problem.

It may simply be waiting for:

- CPUs;
- memory;
- GPUs;
- another running job to finish;
- scheduling priority.

You can inspect the reason using:

```bash
squeue -u $USER
```

---

# Cancel a Job

```bash
scancel JOBID
```

For example:

```bash
scancel 12345
```

---

# Job Output

Our example job contains:

```bash
#SBATCH --output=cbw_test_%j.out
```

`%j` is replaced by the job ID.

For job `12345`, the output file becomes:

```text
cbw_test_12345.out
```

Read it with:

```bash
cat cbw_test_12345.out
```

or:

```bash
less cbw_test_12345.out
```

---

# Common Job Failures

Jobs may finish with states such as:

```text
FAILED
CANCELLED
OUT_OF_MEMORY
TIMEOUT
```

Two particularly important ones are:

### OUT_OF_MEMORY

The job used more memory than requested.

### TIMEOUT

The job ran longer than the requested time.

---

# Choosing Resources

Most bioinformatics jobs require some combination of:

```text
CPU
RAM
Time
```

Some analyses also use:

```text
GPU
```

For example:

```bash
#SBATCH --time=01:00:00
#SBATCH --cpus-per-task=4
#SBATCH --mem=8G
```

requests:

| Resource | Amount |
|---|---:|
| Runtime | 1 hour |
| CPUs | 4 |
| RAM | 8 GB |

---

# More Resources Do Not Always Mean Faster

Suppose a compute node contains:

```text
64 CPU cores
```

but your program only uses:

```text
4 threads
```

Requesting:

```bash
#SBATCH --cpus-per-task=64
```

will not automatically make it faster.

A better request is:

```bash
#SBATCH --cpus-per-task=4
```

A useful rule is:

> **Request resources based on what your application can actually use.**

---

# A Bioinformatics Batch Job

For example:

```bash
#!/bin/bash

#SBATCH --job-name=flagstat
#SBATCH --time=00:10:00
#SBATCH --cpus-per-task=2
#SBATCH --mem=2G
#SBATCH --output=flagstat_%j.out

module load samtools

samtools flagstat example.bam > flagstat.txt
```

Submit it with:

```bash
sbatch flagstat.sh
```

---

# 7. Interactive Computing

Not every task is well suited to a batch job.

Interactive computing is useful when you want to:

- explore data;
- test commands;
- debug code;
- use notebooks;
- interact with R or Python.

We will use two approaches:

1. JupyterHub
2. `salloc`

---

# JupyterHub

JupyterHub provides a browser-based interactive environment.

It can be particularly useful for:

- Python;
- R;
- Jupyter notebooks;
- data visualization;
- exploratory analysis.

Open the workshop JupyterHub page:

**[CBW JupyterHub](JUPYTERHUB_LINK)**

Log in using your training cluster username and password.

---

# Jupyter Environment

Once logged in, you can work with notebooks through your web browser.

A notebook may contain:

- executable code;
- text;
- figures;
- analysis results.

For example, in Python:

```python
print("Hello from CBW")
```

Or:

```python
import os

print(os.getcwd())
```

---

# Jupyter Still Uses Cluster Resources

Although you interact with Jupyter through a web browser, computations still run on the HPC infrastructure.

The browser is only the interface.

Conceptually:

```text
Web browser
     |
     v
JupyterHub
     |
     v
Compute resources
     |
     v
Python / R / notebooks
```

---

# Interactive Computing with `salloc`

You can also request resources directly from the terminal.

For example:

```bash
salloc --time=00:30:00 --cpus-per-task=2 --mem=4G
```

This requests:

```text
Time:     30 minutes
CPUs:     2
Memory:   4 GB
```

---

# Check Your Hostname

Before requesting resources:

```bash
hostname
```

Then run:

```bash
salloc --time=00:30:00 --cpus-per-task=2 --mem=4G
```

Check again:

```bash
hostname
```

Depending on the cluster configuration, you may need to start a shell or use `srun` after the allocation. The instructor will demonstrate the configuration used by this training cluster.

The key idea is that Slurm has allocated compute resources for your interactive work.

---

# Run Commands Interactively

For example:

```bash
module load samtools
```

Then:

```bash
samtools --version
```

Or start Python:

```bash
python
```

---

# Finish the Interactive Session

When you are finished:

```bash
exit
```

Always release interactive resources when you no longer need them.

---

# Batch or Interactive?

| Task | Recommended Approach |
|---|---|
| Long analysis | Batch job |
| Repetitive analysis | Batch job |
| Production workflow | Batch job |
| Quick test | Interactive |
| Debugging | Interactive |
| Jupyter notebook | JupyterHub |
| Exploring data | Interactive/JupyterHub |

In general:

> **Use interactive resources to explore and test. Use batch jobs to run production analyses.**

---

# 8. Bioinformatics Mini-Challenge

Now combine everything from the workshop.

Suppose the following files are available:

```text
reads.fastq.gz
reference.fa
```

Your task is to calculate basic sequence statistics using `seqkit`.

---

# Step 1 — Create a Working Directory

```bash
mkdir -p ~/CBW/results
```

Move into the workshop directory:

```bash
cd ~/CBW
```

---

# Step 2 — Inspect the Input Files

```bash
ls -lh data/
```

Look at the reference:

```bash
head data/reference.fa
```

Look at the compressed FASTQ:

```bash
zcat data/reads.fastq.gz | head
```

---

# Step 3 — Find the Software

```bash
module spider seqkit
```

Load it:

```bash
module load seqkit
```

Check the version:

```bash
seqkit version
```

---

# Step 4 — Test the Command Interactively

Start an interactive allocation:

```bash
salloc --time=00:10:00 --cpus-per-task=2 --mem=2G
```

Try:

```bash
seqkit stats data/reference.fa
```

Then:

```bash
seqkit stats data/reads.fastq.gz
```

Once you know the command works:

```bash
exit
```

---

# Step 5 — Create a Batch Job

Create:

```text
seqkit_qc.sh
```

with:

```bash
#!/bin/bash

#SBATCH --job-name=seq_qc
#SBATCH --time=00:10:00
#SBATCH --cpus-per-task=2
#SBATCH --mem=2G
#SBATCH --output=seq_qc_%j.out

module load seqkit

seqkit stats data/reads.fastq.gz > results/reads_stats.txt

seqkit stats data/reference.fa > results/reference_stats.txt
```

---

# Step 6 — Submit the Job

```bash
sbatch seqkit_qc.sh
```

---

# Step 7 — Monitor It

```bash
squeue -u $USER
```

---

# Step 8 — Inspect the Results

```bash
cat results/reads_stats.txt
```

and:

```bash
cat results/reference_stats.txt
```

---

# Step 9 — Download Your Results

From your **local computer**, you can use:

```bash
rsync -av username@cbw.c3.ca:~/CBW/results/ .
```

Or download the files using FileZilla.

---

# Congratulations

You have now completed a small HPC bioinformatics workflow:

```text
Register
   |
   v
Log in
   |
   v
Navigate files
   |
   v
Transfer data
   |
   v
Load software
   |
   v
Test interactively
   |
   v
Submit batch job
   |
   v
Monitor job
   |
   v
Inspect results
   |
   v
Transfer results back
```

This same general workflow can be applied to much larger bioinformatics analyses.

---

# Quick Reference

## Connect

```bash
ssh username@cbw.c3.ca
```

---

## Navigate

```bash
pwd
ls -lh
cd DIRECTORY
cd ..
cd ~
```

---

## Manage Files

```bash
mkdir DIRECTORY
cp SOURCE DESTINATION
mv SOURCE DESTINATION
rm FILE
```

---

## Inspect Files

```bash
head FILE
tail FILE
less FILE
wc -l FILE
```

---

## Transfer Files

Upload:

```bash
rsync -av --progress FILE username@cbw.c3.ca:~/
```

Download:

```bash
rsync -av --progress username@cbw.c3.ca:~/FILE .
```

---

## Software

```bash
module spider SOFTWARE
module load SOFTWARE
module list
module purge
```

---

## Submit a Job

```bash
sbatch job.sh
```

---

## Monitor Jobs

```bash
squeue -u $USER
```

---

## Cancel a Job

```bash
scancel JOBID
```

---

## Interactive Resources

```bash
salloc --time=01:00:00 --cpus-per-task=2 --mem=4G
```

---

# The Basic HPC Workflow

```text
                  Your computer
                       |
                       | SSH / file transfer
                       v
                  Login Node
                       |
          +------------+------------+
          |                         |
          | sbatch                  | interactive
          v                         v
       Slurm                     salloc /
          |                      JupyterHub
          |                         |
          +------------+------------+
                       |
                       v
                  Compute Node
                       |
                       v
                    Analysis
                       |
                       v
                     Results
```

---

# One Rule to Remember

> **Use the login node to manage your work. Use compute resources to do the work.**

---

# Workshop Modules

1. [Introduction to HPC Infrastructure](01-hpc-infrastructure.md)
2. [Register and Log In](02-register-login.md)
3. [Browse Your Directory](03-linux-files.md)
4. [Transfer Files In and Out](04-file-transfer.md)
5. [Software Environment](05-software-environment.md)
6. [Submit and Monitor Jobs](06-slurm-jobs.md)
7. [Interactive Computing](07-interactive-computing.md)
8. [Bioinformatics Mini-Challenge](08-mini-challenge.md)

---

# Suggested Repository Structure

```text
cbw-computing-onboarding/
│
├── README.md
│
├── 01-hpc-infrastructure.md
├── 02-register-login.md
├── 03-linux-files.md
├── 04-file-transfer.md
├── 05-software-environment.md
├── 06-slurm-jobs.md
├── 07-interactive-computing.md
├── 08-mini-challenge.md
│
├── exercises/
│   ├── linux/
│   ├── transfer/
│   ├── slurm/
│   └── mini-challenge/
│
├── scripts/
│   ├── hello_slurm.sh
│   ├── samtools_flagstat.sh
│   └── seqkit_qc.sh
│
├── data/
│   └── README.md
│
├── cheat-sheet.md
│
└── instructor/
    ├── cluster-setup.md
    ├── preparation-checklist.md
    ├── exercise-solutions.md
    └── troubleshooting.md
```
