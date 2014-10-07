.. YeoLab/welcome documentation master file, created by
   sphinx-quickstart on Mon Aug 18 10:36:39 2014.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to YeoLab/welcome's documentation!
==========================================

1. First thing you'll want to do is get an account on TSCC. Email `Jim Hayes`__

2. Next, make sure you ask a drylab person to get added to the YeoLab_ group.

3. Install hub_. If you have a Mac, install via brew_.
4. Clone the ``gscripts`` repository:

    hub clone YeoLab/gscripts



Make a virtual environment
--------------------------

On TSCC, the easiest way to create a virtual evironment (aka `virtualenv`) is by making one off of the `base` environment, which already has a bunch of modules that we use all the time (`numpy`, `scipy`, `matplotlib`, `pandas`, `scikit-learn`, `ipython`, the list goes on). Here's how you do it:

    conda create --clone base --name yourname

Then activate your environment with

    source activate yourname

And exit it with,

    source deactivate

Installing/upgrading packages
-----------------------------

To install Python packages first try `conda install`:

    conda install pandas

If there is no package in conda, then (and ONLY then) try `pip`:

    pip install pandas

Useful things to do on TSCC
---------------------------

Check the status of your jobs:

    qstat -u $(whoami)

If you have installed `gscripts` and you `source`
`gscripts/bashrc/tscc_bash_settings_current`, then you can do the same thing
with `qme`:

    qme

This outputs,

    (olga)[obotvinnik@tscc-login2 ~]$ qme

    tscc-mgr.sdsc.edu:
                                                                                      Req'd    Req'd       Elap
    Job ID                  Username    Queue    Jobname          SessID  NDS   TSK   Memory   Time    S   Time
    ----------------------- ----------- -------- ---------------- ------ ----- ------ ------ --------- - ---------
    2006527.tscc-mgr.local  obotvinnik  home-yeo STDIN             35367     1     16    --   04:00:00 R  02:35:36
    2007542.tscc-mgr.local  obotvinnik  home-yeo STDIN              6168     1      1    --   08:00:00 R  00:28:08
    2007621.tscc-mgr.local  obotvinnik  home-yeo STDIN               --      1     16    --   04:00:00 Q       --


.. _Jim Hayes: jhayes@sdsc.edu
.. _YeoLab: http://github.com/YeoLab
.. _brew: http://brew.sh
.. _hub: https://hub.github.com/


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

