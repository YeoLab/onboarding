.. YeoLab/welcome documentation master file, created by
   sphinx-quickstart on Mon Aug 18 10:36:39 2014.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to YeoLab/welcome's documentation!
==========================================

1. First thing you'll want to do is get an account on TSCC. Email `Jim Hayes`_

2. Next, make sure you ask a drylab person to get added to the YeoLab_ group
on TSCC (called ``yeo-group``).

3. If you don't have a Github_ account yet, go make one.

4. Ask to get added to the YeoLab group on Github.

5. Install hub_. If you have a Mac, install via brew_.
6. Clone the ``gscripts`` repository:

.. code::

    hub clone YeoLab/gscripts

7. Install PyCharm_ (Community edition is fine).

Make a virtual environment on TSCC
----------------------------------

On TSCC, the easiest way to create a virtual evironment (aka `virtualenv`) is by making one off of the `base` environment, which already has a bunch of modules that we use all the time (`numpy`, `scipy`, `matplotlib`, `pandas`, `scikit-learn`, `ipython`, the list goes on). Here's how you do it:

.. code::

    conda create --clone base --name yourname

Then activate your environment with

.. code::

    source activate yourname

And exit it with,

.. code::

    source deactivate

Installing/upgrading packages
-----------------------------

To install Python packages first try `conda install`:

.. code::

    conda install pandas

If there is no package in conda, then (and ONLY then) try `pip`:

.. code::

    pip install pandas

Useful things to do on TSCC
---------------------------

Submit jobs:

.. code::

    qstat -q home-yeo -l nodes=1:ppn=2 -l walltime=0:30:00 myscript.sh

Submit an interactive job:

.. code::

    qstat -I -q home-yeo -l nodes=1:ppn=2 -l walltime=0:30:00

Check the status of your jobs:

.. code::

    qstat -u $(whoami)

If you have installed ``gscripts`` and you ``source``
``gscripts/bashrc/tscc_bash_settings_current``, then you can do the same thing
with ``qme``:

.. code::

    qme

This outputs,

.. code::

    (olga)[obotvinnik@tscc-login2 ~]$ qme

    tscc-mgr.sdsc.edu:
                                                                                      Req'd    Req'd       Elap
    Job ID                  Username    Queue    Jobname          SessID  NDS   TSK   Memory   Time    S   Time
    ----------------------- ----------- -------- ---------------- ------ ----- ------ ------ --------- - ---------
    2006527.tscc-mgr.local  obotvinnik  home-yeo STDIN             35367     1     16    --   04:00:00 R  02:35:36
    2007542.tscc-mgr.local  obotvinnik  home-yeo STDIN              6168     1      1    --   08:00:00 R  00:28:08
    2007621.tscc-mgr.local  obotvinnik  home-yeo STDIN               --      1     16    --   04:00:00 Q       --

Check the status of your array jobs:

.. code::

    qstat -t

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

.. _Jim Hayes: jhayes@sdsc.edu
.. _YeoLab: http://github.com/YeoLab
.. _brew: http://brew.sh
.. _hub: https://hub.github.com/
.. _Github: http://github.com/
.. _PyCharm: http://www.jetbrains.com/pycharm/

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

