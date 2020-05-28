# TSCC Onboarding

**Authors**: Clarence Mah, Brian Yee<br>
**Last Updated**: 5-07-20

## About TSCC

The Triton Shared Computing Cluster (TSCC) houses our 640-core supercomputer for all our computing needs. While this guide covers most of the essentials, see the [official TSCC user guide](https://www.sdsc.edu/support/user_guides/tscc.html) for a much more comprehensive overview.

The main contacts for questions about TSCC are the `dry lab` and `TSCC users` mailing lists. The main contact for problems with TSCC is [Eva Hocks](tscc-support@sdsc.edu).

# Additional Guides

[Jupyter notebooks on TSCC](TSCC%20Onboarding/Jupyter%20notebooks%20on%20TSCC.md)

[Manage your compute jobs on TSCC](TSCC%20Onboarding/Manage%20your%20compute%20jobs%20on%20TSCC.md)

[Access TSCC files locally with SSHFS](TSCC%20Onboarding/Access%20TSCC%20files%20locally%20with%20SSHFS.md)

[Make a virtual conda environment on TSCC](TSCC%20Onboarding/Make%20a%20virtual%20conda%20environment%20on%20TSCC.md)

[Running the eCLIP Pipeline on TSCC](TSCC%20Onboarding/Running%20the%20eCLIP%20Pipeline%20on%20TSCC.md)

# Initial Setup

Your first login session should include some of the following commands, which will familiarize you with the cluster, and help you setup common properties.

## Log on!

First, log in to TSCC!

```bash
ssh YOUR_TSCC_USERNAME@tscc-login2.sdsc.edu
```

TSCC has two login nodes, `login1` and `login2` for load-balancing (i.e. so if you just log on to `tscc.sdsc.edu`, it’ll choose whichever login node is less occupied. We’re logging in to a specific node because then we’ll always have our screen session on the same node) This is logging specifically on to `login2`. You can do `login1` if you like, as well, to balance it out :)

## Organize your home directory

Your data and analyses go in these directories:

- `/home/$USER`: your home directory (100 GB)
- `/projects/ps-yeolab3/$USER`: your shared lab directory (permanent storage)
- `/projects/ps-yeolab4/seqdata`: where all sequencing data is stored unless otherwise specified

    ### All raw data (fastqs, BCLs, ABFs etc.) should reside in `seqdata` and should be considered **permanent**. **You should NOT delete anything from `seqdata`!**

    **Note:** Data should not be processed in `seqdata`. Processing should happen in `scratch`.

- `/oasis/tscc/scratch/$USER`: your virtually unlimited temporary storage for storing intermediate files and outputs from compute jobs (800+ TBs of SSDs, shared). **Optimized for faster read/write**, so please try to do most of your processing here (and transfer to permanent storage later)!

    **Note:** Anything saved here is subject to deletion without warning after 3 months or less of storage. In particular, the large `.sam` and `.bam` files can get deleted, and you will have to re-run your analysis.

Let's link these directories for easy access from your home directory:

```bash
ln -s /projects/ps-yeolab3/$USER $HOME/projects
ln -s /projects/ps-yeolab4/seqdata $HOME/seqdata
ln -s /oasis/tscc/scratch/$USER $HOME/scratch
```

## Let us see your stuff

Make everything readable by other Yeo lab members and restrict access from other users

```
chmod -R g+r ~/
chmod -R g+r ~/scratch/
chmod -R o-rwx ~/
chmod -R o-rwx ~/scratch/
```

But `git` will get mad at you if your ~/.ssh keys private keys are visible by others, so make them visible to only you via:

```
chmod -R go-rwx ~/.ssh/
```

## Add lab software

- `modules` are system-wide installations of programs and pipelines that are frequently used in the lab. However in order to access these modules, you’ll need to specify the path to where they exist.
- Add the following two lines to your `.bashrc` file:

```
module use /projects/ps-yeolab4/modulefiles
module load yeolabmodules/0.0.2
```

- source the `.bashrc` file to apply the above changes to your environment variables we’ve created.

```
source ~/.bashrc
```

- Test that you can load and unload a module (typing `module avail` will list every module we have):

```
module load samtools
module list
module unload samtools
```

## Add reference genomes

To run the analysis pipeline, you will need to specify where the genomes are on TSCC, and you can do this by adding this line to your `~/.bashrc`:

```
GENOME=/projects/ps-yeolab4/genomes
```

## Manage your terminal sessions with `Screen`

`Screen` is an awesome tool which allows you to have multiple “tabs” open in the same login session, and you can easily transition between screens. Plus, they’re persistent, so you can leave something running in a screen session, log out of TSCC, and it will still be running! Amazing!

### Configure your `screen`

![http://byee4.github.io/img/screen.png](http://byee4.github.io/img/screen.png)

To get a nice status bar at the bottom of your terminal window like this, get this `.screenrc` file:

```bash
cd # change directory to home
wget https://raw.githubusercontent.com/olgabot/rcfiles/master/.screenrc
```

**Note:** The control letter is `j`, not `a` in the documentation above, so for example to create a new window, do `Ctrl-j c` and to kill the current window, do `Ctrl-j k`. Do `Ctrl-j j` to switch between windows, and `Ctrl-j #`, where # is some window number, to switch to a numbered tab specifically (e.g. `Ctrl-j 2` to switch to tab number 2.

### Using `screen`

Now to start a screen session do:

```
screen
```

If you’re re-logging in and you have an old screen session, do this to “re-attach” the screen window.

```
screen -x
```

Every time you log in to TSCC, you’ll want to reattach the screens from before, so the first step I always take when I log in to TSCC is exactly that, `screen -x`.

## Download and install Anaconda

Download the Anaconda package manager using wget (web-get). The link below is from the Anaconda downloads page. This will be useful for installing and managing Python packages, R packages, and programs you will be using.

```
wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh
```

To install Anaconda, run the shell script with bash (this will take some time). It will ask you a bunch of questions, and use the defaults for them (press enter for all).

```
bash Anaconda3-2019.10-Linux-x86_64.sh
```

To activate Anaconda, source your .bashrc:

```
source ~/.bashrc
```

Make sure your Python is point to the Anaconda python with:

```
which python
```

The output should look something like:

```
~/anaconda3/bin/python
```
