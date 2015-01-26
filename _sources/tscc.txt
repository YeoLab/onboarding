TSCC
====

TSCC_ houses our 640-core supercomputer as part of a condo resource sharing
system which allows other researchers (mostly bioinformaticians from the
Ideker, Ren, Zhang labs) to use our portion for their jobs (with an 8 hour
time limit) which allows us to use their portions when we need extra
computing power. We lead the pack in terms of pure crunching power,
with the Ideker lab in close second place with 512 cores. But they have 2x
the RAM per core for jobs that require lots of memory,
so our purchases are complimentary and sharing is encouraged.

The main contacts for questions about TSCC are the `dry lab`_ and
`TSCC users`_ mailing lists. The main contact for problems with TSCC is `Jim Hayes`_

Important rules
---------------

.. warning::

    1. All sequencing data is stored in the ``/projects/ps-yeolab/seqdata`` folder
    2. The folder ``seqdata`` is intended as permanent storage and no folders
       or files there should ever be deleted
    3. Do not process data in ``seqdata``. Use the directory structure
       described in `Organize your home directory`_ to create a ``scratch``
       folder for all data processing.

First Steps
-----------

Your first login session should include some of the following commands,
which will familiarize you with the cluster, teach you how to do some useful
tasks on the queue, and help you set up a common directory structure shared
by everyone in the lab.

Log on!
~~~~~~~

First, log in to TSCC!

.. code::

    ssh YOUR_TSCC_USERNAME@tscc-login2.sdsc.edu

TSCC has two login nodes, ``login1`` and ``login2`` for load-balancing (i.e.
so if you just log on to ``tscc.sdsc.edu``, it'll choose whichever login
node is less occupied. We're logging in to a specific node because then
we'll always have our screen session on the same node) This is logging
specifically on to ``login2``. You can do ``login1`` if you like, as well,
to balance it out :)

Start a screen session
~~~~~~~~~~~~~~~~~~~~~~

Screen_ is an awesome tool which allows you to have multiple "tabs" open in
the same login session, and you can easily transition between screens. Plus,
they're persistent, so you can leave something running in a screen session,
log out of TSCC, and it will still be running! Amazing! If you have
suggestions of things to add to this ``.screenrc``, feel free to make a pull
request on Olga's rcfiles_ github repo.

To get a nice status bar at the bottom of your terminal window, get this
``.screenrc`` file:

.. code::

    cd
    wget https://raw.githubusercontent.com/olgabot/rcfiles/master/.screenrc

.. note::

    The control letter is ``j``, not ``a`` in the documentation above,
    so for example to create a new window, do ``Ctrl-j c`` and to kill the
    current window, do ``Ctrl-j k``. Do ``Ctrl-j j`` to switch between
    windows, and ``Ctrl-j #``, where # is some window number,
    to switch to a numbered tab specifically (e.g. ``Ctrl-j 2`` to switch to
    tab number 2.

This ``.screenrc`` adds a status bar at the bottom of your screen, like this:

.. image:: screen_status.png

Now to start a screen session do:

.. code::

    screen

If you're re-logging in and you have an old screen session,
do this to "re-attach" the screen window.

.. code::

    screen -x

Every time you log in to TSCC, you'll want to reattach the screens from
before, so the first step I always take when I log in to TSCC is exactly
that, ``screen -x``.

Get ``gscripts`` access to software
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

0. Before you're able to clone the gscripts github repo, you'll need to add
   your ssh keys on TSCC to your Github account. Follow `Github's instructions
   for generating SSH keys`_.

1. First, clone the ``gscripts`` github repo to your home directory on TSCC
   (this assumes you've already created a github account).

.. code::

    # <on TSCC>
    cd
    git clone http://github.com/YeoLab/gscripts

2. Add this line to the **end** of your ``~/.bashrc`` file (using either
   ``emacs`` or ``vi``/``vim``, your choice)

.. code::

    source ~/gscripts/bashrc/tscc_bash_settings_current

3. "source" the ``.bashrc`` file so you load all the convenient environment
    variables we've created.

.. code::

    source ~/.bashrc

Make a virtual environment on TSCC
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

On TSCC, the easiest way to create a virtual evironment (aka ``virtualenv``)
is by making one off of the ``base`` environment, which already has a bunch of
modules that we use all the time (``numpy``, ``scipy``, ``matplotlib``, ``pandas``, ``scikit-learn``, ``ipython``, the list goes on). Here's how you do it:

.. note::

    The command ``$USER`` is meant to be literal, meaning you can exactly copy
    the below command, and TSCC will create an environment with your username.
    If you don't believe me, compare the output of:

    .. code::

        echo USER

    to the output of:

    .. code::

        echo $USER

    The second one should output your TSCC username, because the ``$`` dollar
    sign indicates to the shell that you're asking for the variable ``$USER``,
    not the literal word "USER".

.. code::

    conda create --clone base --name $USER

.. note::
    You can also create an environment from scratch using ``conda`` to install
    all the Anaconda Python packages, and then using ``pip`` in the environment
    to install the remaining packages, like so:

    .. code::

        conda create --yes --name ENVIRONMENT_NAME pip numpy scipy cython matplotlib nose six scikit-learn ipython networkx pandas tornado statsmodels setuptools pytest pyzmq jinja2 pyyaml pymongo biopython
        source activate ENVIRONMENT_NAME
        pip install seaborn fastcluster gspread brewer2mpl husl semantic_version joblib pybedtools gffutils matplotlib-venn HTSeq misopy
        pip install https://github.com/YeoLab/clipper/tarball/master
        pip install https://github.com/YeoLab/gscripts/tarball/master
        pip install https://github.com/YeoLab/flotilla/tarball/master

    These commands is how the ``base`` environment was created.

Then activate your environment with

.. code::

    source activate $USER

You'll probably stay in this environment all the time.

.. warning::

    Make sure to add ``source activate $USER`` to your ``~/.bashrc`` file!
    Then you will always be in your environment

If you need to switch to another environment, then exit your environment with:

.. code::

    source deactivate

.. note::

    Now that you've created your own environment, go to your gscripts folder
    and install your own personal gscripts, to make sure it's the most updated
    version.

    .. code::

        cd ~/gscripts
        pip install .  # The "." means install "this," as in "this folder where I am"

Add the location of ``GENOME`` to your ``~/.bashrc``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
To run the analysis pipeline, you will need to specify where the genomes are
on TSCC, and you can do this by adding this line to your ``~/.bashrc``:

.. code::

    GENOME=/projects/ps-yeolab/genomes

Organize your home directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create an organized ``home`` directory structure following a common
template, so others can find your scripts, workflows,
and even final results/papers!  Do not store actual data in your home
directory as is is limited to 100 GB only.



Link your scratch directory to your home
++++++++++++++++++++++++++++++++++++++++

The "``scratch``" storage on TSCC is for temporary (after 90 days it gets
purged) storage. It's very useful for storing intermediate files,
and outputs from compute jobs because the data there is stored on
solid-state drives (SSDs, currently 300TB) which have incredibly fast
read-write speeds, which is perfect for outputs from alignment algorithms.
It can be annoying to go back and forth between your scratch directory,
so it's convenient to have a link to your scratch from home,
which you can make like this:

.. code::

   ln -s /oasis/tscc/scratch/$USER $HOME/scratch

.. note::

    This is virtually unlimited temporary storage space,
    designed for heavy I/O.  Aside from common reference files (e.g.
    Genomes, GENCODE, etc.) this should be the only space that you can
    read/write to from your scripts/workflows! The '''parallel''' throughput
    of this storage is 100 GB/s (e.g. 10 tasks can each read/write at 10
    GB/s at the same time)

.. warning::

    Anything saved here is subject to deletion without warning after 3 months
    or less of storage. In particular, the large ``.sam`` and ``.bam`` files
    can get deleted, even though their ``.done`` files (produced by the
    GATK Queue RNA-seq pipeline as a placeholder) will still exist, and they
    will seem done to the pipeline. To avoid lost data, here are a few steps:

    1. Keep your metadata sample/cell counts are in your ``$HOME/projects`` or
       ``/projects/ps-yeolab/$USER`` folder, which don't get purged
       periodically.
    2. Delete ``*.done`` files when re-rerunning a partially eroded pipeline
       run.
    3. Use this recursive touch command to "refresh" the decay clock on your
       files before important meetings and re-analysis steps:

       .. code::

            cd important_scratch_dir
            find . | xargs touch

Create workflow and projects folders
++++++++++++++++++++++++++++++++++++

Create ``~/workflows`` for your personal bash, makefile, queue, and so on,
scripts, before you add them to gscripts, and ``~/projects`` for your
projects to organize figures, notebooks, final results, and even manuscripts.

.. code::

    mkdir ~/workflows ~/projects

Here's an example project directory structure:

.. code::

    $ ls -lha /home/gpratt/projects/fox2_iclip/
    total 9.5K
    drwxr-xr-x  2 gpratt yeo-group  5 Sep 16  2013 .
    drwxr-xr-x 40 gpratt yeo-group 40 Nov 24 12:20 ..
    lrwxrwxrwx  1 gpratt yeo-group 49 Aug 21  2013 analysis -> /home/gpratt/scratch/projects/fox2_iclip/analysis
    lrwxrwxrwx  1 gpratt yeo-group 45 Aug 21  2013 data -> /home/gpratt/scratch/projects/fox2_iclip/data
    lrwxrwxrwx  1 gpratt yeo-group 50 Aug 21  2013 scripts -> /home/gpratt/processing_scripts/fox2_iclip/scripts

.. note::

    Notice that all of these are soft-links to either ``~/scratch`` or some
    other processing scripts.

Let us see your stuff
+++++++++++++++++++++

Make everything readable by other yeo lab members and restrict access from
other users (per HIPAA/HITECH requirements)

.. code::

    chmod -R g+r ~/
    chmod -R g+r ~/scratch/
    chmod -R o-rwx ~/
    chmod -R o-rwx ~/scratch/

But ``git`` will get mad at you if your ~/.ssh keys private keys are visible
by others, so make them visible to only you via:

.. code::

    chmod -R go-rwx ~/.ssh/

In the end, your '''home''' directory should look something like this:

.. code::

    $ ls -l $HOME
    lrwxrwxrwx  1 bkakarad yeo-group    29 Jun 24  2013 scratch -> /oasis/tscc/scratch/bkakarad/
    drwxr-x---+ 2 bkakarad yeo-group     2 Jun 24  2013 gscripts
    drwxr-x---+ 3 bkakarad yeo-group     3 Jun 24  2013 projects
    drwxr-x---+ 2 bkakarad yeo-group     2 Jun 24  2013 workflows

Share your Dropbox account for easy figure syncing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Installing and upgrading Python packages
----------------------------------------

To install Python packages first try ``conda install``:

.. code::

    conda install <package name>

If there is no package in conda, then (and ONLY then) try `pip`:

.. code::

    pip install <package name>

To upgrade packages, do:

(using ``conda``)

.. code::

    conda update <package name>

(using ``pip``)

.. code::

    pip install -U <package name>


Submitting and managing compute jobs on TSCC
--------------------------------------------

Submit jobs
~~~~~~~~~~~

To submit a script that you wrote, in this case called ``myscript.sh``,
to TSCC, do:

.. code::

    qsub -q home-yeo -l nodes=1:ppn=2 -l walltime=0:30:00 myscript.sh

Submit interactive jobs
~~~~~~~~~~~~~~~~~~~~~~~

To submit interactive jobs, do:

.. code::

    qsub -I -q home-yeo -l nodes=1:ppn=2 -l walltime=0:30:00

Submit jobs to ``home-scrm``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To submit to the ``home-scrm`` queue, add ``-W group_list=scrm-group`` to
your ``qsub`` command:

.. code::

    qsub -I -l walltime=0:30:00 -q home-scrm -W group_list=scrm-group


Submitting many jobs at once
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have a bunch of commands you want to run at once,
you can use this script to submit them all at once. In the next example,
``commands.sh`` is a file has the commands you want on their own line,
i.e. one command per line.

.. code::

    java -Xms512m -Xmx512m -jar /home/yeo-lab/software/gatk/dist/Queue.jar \
    -S ~/gscripts/qscripts/do_stuff.scala --input commands.sh -run -qsub \
    -jobQueue <queue> -jobLimit <n> --ncores <n> --jobname <name> -startFromScratch

This runs a scala job that submits sub-jobs to the PBS queue under name you
fill in where <name> now sits as a placeholder.

Check job status, aka "why is my job stuck?"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check the status of your jobs:

.. code::

    qme

.. note:: This will only work if you have followed instructions and have
``source``'d the ``~/gscripts/tscc_bash_settings_current``  :)

``qme`` outputs,

.. code::

    (olga)[obotvinnik@tscc-login2 ~]$ qme

    tscc-mgr.sdsc.edu:
                                                                                      Req'd    Req'd       Elap
    Job ID                  Username    Queue    Jobname          SessID  NDS   TSK   Memory   Time    S   Time
    ----------------------- ----------- -------- ---------------- ------ ----- ------ ------ --------- - ---------
    2006527.tscc-mgr.local  obotvinnik  home-yeo STDIN             35367     1     16    --   04:00:00 R  02:35:36
    2007542.tscc-mgr.local  obotvinnik  home-yeo STDIN              6168     1      1    --   08:00:00 R  00:28:08
    2007621.tscc-mgr.local  obotvinnik  home-yeo STDIN               --      1     16    --   04:00:00 Q       --

Check job status of array jobs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check the status of your array jobs, you need to specify ``-t`` to see the
status of the individual array pieces.

.. code::

    qstat -t


Killing jobs
~~~~~~~~~~~~

If you have a job you want to stop, kill it with ``qdel JOBID``, e.g.

.. code::

    qdel 2006527

Kill an array job
~~~~~~~~~~~~~~~~~

If the job is an array job, you'll need to add brackets, like this:

.. code::

    qdel 2006527[]


Kill all your jobs
~~~~~~~~~~~~~~~~~~

To kill all the jobs that you've submitted, do:

.. code::

    qdel $(qselect -u $USER)


Which queue do I submit to? (check status of queues)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check the status of the queue (so you know which queues to NOT submit to!)

.. code::

    qstat -q

Example output is,

.. code::

    (olga)[obotvinnik@tscc-login2 ~]$ qstat -q

    server: tscc-mgr.local

    Queue            Memory CPU Time Walltime Node  Run Que Lm  State
    ---------------- ------ -------- -------- ----  --- --- --  -----
    home-dkeres        --      --       --      --    2   0 --   E R
    home-komunjer      --      --       --      --    0   0 --   E R
    home-ong           --      --       --      --    2   0 --   E R
    home-tg            --      --       --      --    0   0 --   E R
    home-yeo           --      --       --      --    3   1 --   E R
    home-visres        --      --       --      --    0   0 --   E R
    home-mccammon      --      --       --      --   15  29 --   E R
    home-scrm          --      --       --      --    1   0 --   E R
    hotel              --      --    168:00:0   --  232  26 --   E R
    home-k4zhang       --      --       --      --    0   0 --   E R
    home-kkey          --      --       --      --    0   0 --   E R
    home-kyang         --      --       --      --    2   1 --   E R
    home-jsebat        --      --       --      --    1   0 --   E R
    pdafm              --      --    72:00:00   --    1   0 --   E R
    condo              --      --    08:00:00   --   18   6 --   E R
    gpu-hotel          --      --    336:00:0   --    0   0 --   E R
    glean              --      --       --      --   24  75 --   E R
    gpu-condo          --      --    08:00:00   --   16  36 --   E R
    home-fpaesani      --      --       --      --    4   2 --   E R
    home-builder       --      --       --      --    0   0 --   E R
    home               --      --       --      --    0   0 --   E R
    home-mgilson       --      --       --      --    0   4 --   E R
    home-eallen        --      --       --      --    0   0 --   E R
                                                   ----- -----
                                                     321   180

So right now is not a good time to submit to the ``hotel`` queue,
since it has a bunch of both running and queued jobs!

Show available "Service Units"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

List the available Service Units (1 SU = 1 core*hour) ... for a quick ego
boost. Also note that our supercomputer is separated in two: yeo-group and
scrm-group, but the total balance is 5.29 million SU, just enough secure us
the top honors :-)

.. code::

    gbalance | sort -nrk 3 | head

    Id Name                 Amount  Reserved Balance CreditLimit Available
    -- -------------------- ------- -------- ------- ----------- ---------
    19 tideker-group        5211035    27922 5183113           0   5183113
    82 yeo-group            3262925        0 3262925           0   3262925
    81 scrm-group           2039328        0 2039328           0   2039328
    14 mgilson-group         663095   208000  455095           0    455095
    73 nanosprings-ucm       650000        0  650000           0    650000
    17 kkey-group            635056     7104  627952           0    627952
    16 k4zhang-group         534430        0  534430           0    534430

List the available TORQUE queues, for a quick boost in motivation!

.. code::

    qstat -q

    Queue            Memory CPU Time Walltime Node  Run Que Lm  State
    ---------------- ------ -------- -------- ----  --- --- --  -----
    home-tideker       --      --       --       16   1   0 --   E R
    home-visres        --      --       --        1   0   0 --   E R
    hotel              --      --    72:00:00   --   25  18 --   E R
    home-k4zhang       --      --       --        4  21   0 --   E R
    home-kkey          --      --       --        5   0   0 --   E R
    pdafm              --      --    72:00:00   --    0   0 --   E R
    condo              --      --    08:00:00   --    0   0 --   E R
    glean              --      --       --      --    0   0 --   E R
    home-builder       --      --       --        8   0   0 --   E R
    home               --      --       --      --    0   0 --   E R
    home-ewyeo         --      --       --       15   0   0 --   E R
    home-mgilson       --      --       --        8   0   0 --   E R
                                               ----- -----
                                                  47    18

Show available processors
~~~~~~~~~~~~~~~~~~~~~~~~~

To show available processors, do

.. code::

    showbf

Show specs of all nodes
~~~~~~~~~~~~~~~~~~~~~~~

.. code::

    pbsnodes -a


IPython notebooks on TSCC
-------------------------

1. To set up IPython notebooks on TSCC, you will want to add some ``alias``
variables to your ``~/.bashrc``. First, on your personal computer,
you will want to set up
`passwordless ssh`_ from your laptop to TSCC. On my laptop,
I have this alias in my `~/.bashrc` file:

.. code::

    IPYNB_PORT=[some number above 1024]
    alias tscc='ssh obotvinnik@tscc-login2.sdsc.edu'

This way, I can just type ``tscc`` and log onto ``tscc-login2``
**specifically**. It is important for IPython notebooks that you always log
on to the same node. You can use ``tscc-login1`` instead, too,
this is just what I have set up. Just replace my login name
("``obotvinnik``") with yours.

2. Next, type ``tscc`` and log on to the server.

3. On TSCC, add these lines to your ``~/.bashrc`` file.

.. code::

    IPYNB_PORT=[same number as the above IPYNB_PORT]
    alias ipynb="ipython notebook --no-browser --port $IPYNB_PORT --matplotlib inline &"
    alias sshtscc="ssh -NR $IPYNB_PORT:localhost:$IPYNB_PORT tscc-login2 &"

Notice that in ``sshtscc``, I use the same port as I logged in to,
`tscc-login2`. The ampersands "`&`" at the end of the lines tell the computer
to run these processes in the background, which is super useful.

4. Now that you have those set up, start up a ``screen`` session,
which allows you to have something running continuously,
without being logged in.

.. code::

    screen -x

5. In this ``screen`` session, now request an interactive job, e.g.:

.. code::

    qsub -I -l walltime=8:00:00 -q home-yeo -l nodes=1:ppn=8

6. Wait for the job to start.

7. Set up passwordless ssh between the compute nodes and TSCC with:

.. code::

    cat .ssh/id_rsa.pub | ssh tscc-login2 'cat >> .ssh/authorized_keys'

8. Wait for the job to start, then type ``ipynb``, press ``ENTER``,
then ``sshtscc`` and press ``ENTER``. again.

9. Back on your home laptop, type

.. code::

    ssh -NL $IPYNB_PORT:localhost:$IPYNB_PORT YOUR_TSCC_USERNAME@tscc-login2
    .sdsc.edu &

Make sure to replace "``YOUR_TSCC_USERNAME``" with your TSCC login :)

8. On your laptop, type the url ``http://localhost:[IPYNB_PORT]`` and replace
"``IPYNB_PORT``" with your actual numbers of the port you're using.

You should now have IPython notebooks on TSCC!


Software goes in ``/projects/ps-yeolab/software``

Make sure to recursively set group read/write permissions to the software
directory so others can use and update the common software, using:

.. code::

    chmod ug+rw /projects/ps-yeolab/software

If your'e installing something from source and using ``./configure``
and ``make`` and all that, then always set the flag
``--prefix=/projects/ps-yeolab/software`` when you run ``./configure``

.. code::

    ./configure --prefix /projects/ps-yeolab/software

When possible install bins to ``/projects/ps-yeolab/software/bin``

Running qscripts GATK Queue pipelines on TSCC
---------------------------------------------

Example scripts can be found in:

.. code::

    /home/gpratt/templates

e.g. for RNA-Seq look at this gist:
    https://gist.github.com/gpratt/294cbdf553ac4f44648a

Each Queue job requires a manifest file with a list of all files to process, and the genome to process them
on

GATK Queue runs exclusively on TSCC (for now, some paths are hard coded and
Gabe doesn't know scala well enough to un-hardcode them)

Running instructions are documented in the each queue file,
as long as you have a checked out copy of gscripts then

This command will show documentation

.. code::

    java -Xms512m -Xmx512m -jar /home/yeo-lab/software/gatk/dist/Queue.jar -S ~/gscripts/qscripts/analyze_rna_seq.scala

will show documentation

Further documentations can be found at the `GATK Queue website`_


.. note::

    Sometimes the login node kills these jobs, logging into a worker node to run these pipelines is a good \
    idea.
    Also these are long running jobs you should you be in a screen session to run these pipelines

analyze_rna_seq
~~~~~~~~~~~~~~~

The queue script ``analyze_rna_seq.scala`` runs or generates:

1. RNA-SeQC_
2. cutadapt
3. miso
4. OldSplice
5. Sailfish
6. A->I editing predictions
7. bigWig files
8. Counts of reads mapping to repetitive elements
9. Estimates of PCR Duplication

Detailed description of `analyze_rna_seq.scala`_ outputs.

analyze_rna_seq_gently
~~~~~~~~~~~~~~~~~~~~~~

The queue script ``analyze_rna_seq_gently.scala`` runs:

1. RNA-SeQC_
2. ...

.. _TSCC: http://rci.ucsd.edu/computing/index.html
.. _dry lab: dryyeo-l@googlegroups.com
.. _TSCC users: tscc-l@mailman.ucsd.edu
.. _Jim Hayes: jhayes@sdsc.edu
.. _hub: https://hub.github.com/
.. _Screen: https://kb.iu.edu/d/acuy
.. _rcfiles: https://github.com/olgabot/rcfiles
.. _passwordless ssh: http://www.linuxproblem.org/art_9.html
.. _GATK Queue website: http://gatkforums.broadinstitute.org/discussion/1306/overview-of-queue
.. _RNA-SeQC: http://www.broadinstitute.org/cancer/cga/rna-seqc
.. _analyze_rna_seq.scala: analyze_rna_seq
.. _Github's instructions     for generating SSH keys: https://help.github.com/articles/generating-ssh-keys/