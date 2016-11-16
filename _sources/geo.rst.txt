How to upload files to GEO
==========================

Preparing to upload to GEO
--------------------------

This is the most time consuming step because it's very tedious to fill
out the spreadsheet. An example notebook doing the soft links and
getting the checksums for all files is
`here <http://nbviewer.jupyter.org/gist/olgabot/bb9f91996e4f414f431b047ea4a16628>`__.

#. Put all your FASTQ.gz files for a project (e.g. ``singlecell_pnm``),
   into a folder which GEO wants to be called the username you upload
   with, so in our case that's ``geneyeo``. I made soft links into
   ``~/projects/singlecell_pnm/data/geneyeo``
#. Put all your processed data (``.csv``, ``.bed``, ``.wig`` files -
   only the ones you actually used in the paper, you don't have to put
   EVERYTHING) into this GEO folder
   (``~/projects/singlecell_pnm/data/geneyeo``) too.
#. Get the ``md5`` checksum/hash for each file.
#. Fill out the GEO submission spreadsheet with all the metadata,
   protocols, and filenames, and checksums with your files.
   `Here <geo_submission_example.xls>`__ is an example.

The actual uploading: FTP Transfer to NCBI
------------------------------------------

This FTP server sucks a lot and the log in times out after 30 sec, so
make sure you have the password ready in your clipboard. Also, the
server times out after 60 sec so make sure you do these commands right
after another or you'll have to start all over (except for creating
folders).

Step 0: Change to the GEO directory in your TSCC account
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Before you log in to GEO, it's easiest if you're already in the
directory with all the files that you want.

::

    [obotvinnik@tscc-1-50 geneyeo]$ cd ~/projects/singlecell_pnm/data/geneyeo

Step 1: Log in to NCBI.
^^^^^^^^^^^^^^^^^^^^^^^

Use the username/password that GEO provides (not putting this here for
safety reasons).

::

    [obotvinnik@tscc-1-50 geneyeo]$ ftp ftp-private.ncbi.nlm.nih.gov
    Connected to ftp-private.ncbi.nlm.nih.gov (130.14.29.30).
    220-
     Warning Notice!

     You are accessing a U.S. Government information system which includes this
     computer, network, and all attached devices. This system is for
     Government-authorized use only. Unauthorized use of this system may result in
     disciplinary action and civil and criminal penalties. System users have no
     expectation of privacy regarding any communications or data processed by this
     system. At any time, the government may monitor, record, or seize any
     communication or data transiting or stored on this information system.
     ---
     Welcome to the NCBI ftp server! The anonymous access URL is ftp://ftp.ncbi.nlm.nih.gov/

     Public data may be downloaded by logging in as "anonymous" using your E-mail address as a password.

     Please see ftp://ftp.ncbi.nlm.nih.gov/README.ftp for hints on large file transfers
    220 FTP Server ready.
    Name (ftp-private.ncbi.nlm.nih.gov:obotvinnik): geo
    331 Password required for geo
    Password:
    230 User geo logged in
    Remote system type is UNIX.
    Using binary mode to transfer files.

.. note::

    We're now using `ftp` commands, which are all for the `remote` server
    (GEO server), so if you want to locally change directories, like on TSCC,
    you would use `lcd` (local change directory) rather than `cd`, which is
    implied to be for the remote server.

Step 2: Turn off interactive mode
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Turn off interactive prompting for "are you sure you want to upload this
file?" with ``prompt -n``, which requires you to respond within 60s or
you lose your upload.

::

    ftp> prompt n
    Interactive mode off.

Step 3: Look at the files that are there
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Look at the files that are there (at the GEO remote host) with ``ls``:

::

    ftp> ls
    227 Entering Passive Mode (130,14,29,30,196,30).
    150 Opening BINARY mode data connection for file list
    drwxrwsr-x   2 geo      geo          4096 Aug 10 15:32 :aleciajanetwigger
    drwxrwsr-x   2 geo      geo          4096 Aug  9 01:15 :etkoishi@gmail.com
    -rw-rw-r--   1 geo      geo      1179149954 Aug 10 20:58 CVN_01_R1.fastq.gz
    -rw-rw-r--   1 geo      geo      1390825808 Aug 10 21:02 CVN_02_R1.fastq.gz
    -rw-rw-r--   1 geo      geo         95232 Aug  9 07:07 GEO spreadsheet_epigenetic_inheritance.xls
    drwxrwsr-x   2 geo      geo          4096 Aug  8 14:01 Jagan_Pongubala
    drwxrwsr-x   2 geo      geo          4096 Aug  4 18:50 LL2697
    drwxrwsr-x   7 geo      geo          4096 Aug 10 20:46 NEW_SUBMISSIONS
    drwxrwsr-x   3 geo      geo          4096 Aug 10 14:28 Ping_Wu
    drwxrwsr-x   2 geo      geo          4096 Aug  8 14:01 Rio_GEO_2
    drwxrwsr-x   3 geo      geo          4096 Jul 30 03:55 Rio_GEO_3
    drwxrwsr-x   2 geo      geo          4096 Aug 10 14:40 Users
    drwxrwsr-x   3 geo      geo          4096 Aug  8 12:24 dmemoli
    drwxrwsr-x 138 root     geo        368640 Aug 10 21:31 fasp
    drwxrwsr-x   2 geo      geo          4096 Aug 10 04:25 fslab
    drwxrwsr-x   2 geo      geo          4096 Aug 10 20:24 geneyeo
    drwxrwsr-x   4 geo      geo          4096 Aug  1 17:21 jagan_pongubala@orcid
    drwxrwsr-x   2 geo      geo          4096 Aug  7 14:09 jinhyungcho
    drwxrwsr-x   2 geo      geo          4096 Aug  3 20:47 johannmignolet
    drwxrwsr-x   2 geo      geo          4096 Aug 10 04:24 mengzhang
    drwxrwsr-x   2 geo      geo          4096 Aug  7 14:17 rahulsinha
    drwxrwsr-x   2 4459     geo          4096 Jul 20 14:43 raw
    drwxrwsr-x   5 geo      geo          4096 Aug  9 22:10 sgrann_100816
    drwxrwsr-x   2 geo      geo          4096 Aug  7 14:16 sulevk_hypotrohpy
    drwxrwsr-x   2 geo      geo          4096 Aug  8 14:01 tbasika
    drwxrwsr-x   2 geo      geo          4096 Aug  4 13:57 vlabunskyy@partners.org
    drwxrwsr-x   2 geo      geo          4096 Aug 10 04:22 wangzhonghua2016
    drwxrwsr-x   2 geo      geo          4096 Aug  3 07:58 zheda_intestinalcancer
    drwxrwsr-x   3 geo      geo          4096 Aug  7 14:04 zjjcom682161
    226 Transfer complete

Haha there's a bunch of people who didn't follow instructions and
uploaded here (including me ...)

Step 4: Change to ``fasp`` directory
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Change to the ``fasp`` directory they want you to use:

::

    ftp> cd fasp
    250 CWD command successful

Step 5: Make a directory with Gene's username
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FTP can't copy directory structure, only files, so we have to make a
directory manually. They want it with the uploader's username, so make a
directory with Gene's username.

::

    ftp> mkdir geneyeo
    257 "/fasp/geneyeo" - Directory successfully created

Step 6: Change to the new directory
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Change to the new directory

::

    ftp> cd geneyeo
    250 CWD command successful

Step 7: Upload all the files
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You'll next do a "multiple-file put" of all the files that are in your
TSCC ``geneyeo`` directory to put on GEO's ``geneyeo`` directory. The
command is ``mput *``, and example output is below.

::

    ftp> mput *
    local: CVN_01_R1.fastq.gz remote: CVN_01_R1.fastq.gz
    227 Entering Passive Mode (130,14,29,35,195,175).
    150 Opening BINARY mode data connection for CVN_01_R1.fastq.gz
    226 Transfer complete
    1179149954 bytes sent in 184 secs (6399.62 Kbytes/sec)
    local: CVN_02_R1.fastq.gz remote: CVN_02_R1.fastq.gz
    227 Entering Passive Mode (130,14,29,35,196,67).
    150 Opening BINARY mode data connection for CVN_02_R1.fastq.gz
    226 Transfer complete
    1390825808 bytes sent in 60.8 secs (22857.59 Kbytes/sec)
    local: CVN_03_R1.fastq.gz remote: CVN_03_R1.fastq.gz
    227 Entering Passive Mode (130,14,29,35,196,214).
    150 Opening BINARY mode data connection for CVN_03_R1.fastq.gz
    226 Transfer complete
    1381847754 bytes sent in 154 secs (8968.02 Kbytes/sec)
    local: CVN_04_R1.fastq.gz remote: CVN_04_R1.fastq.gz
    227 Entering Passive Mode (130,14,29,35,196,74).
    150 Opening BINARY mode data connection for CVN_04_R1.fastq.gz


Telling NCBI you uploaded stuff
-------------------------------

After your transfer is complete, you need to tell the NCBI so

    After file transfer is complete, please `e-mail <geo@ncbi.nlm.nih.gov>`_ GEO with the following information:
    - GEO account username (olga.botvinnik@gmail.com);
    - Names of the directory and files deposited;
    - Public release date (required - up to 3 years from now - see FAQ).
