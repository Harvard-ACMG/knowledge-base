## Visualizing output on Cannon (formerly Odyssey)

**NOTE: The Odyssey cluster was renamed to Cannon in September 2019.**

Traditionally, GEOS-chem users have relied on IDL-based software, such
as GAMAP, for data analysis and visualization. But to many GEOS-Chem
user groups, the IDL software has become prohibitively expensive.
Several members of the GEOS-Chem community have begun to develop file
software programs in the Python language for reading, visualizing, and
processing GEOS-Chem data. Python is a free and open-source computer
language that comes with several pre-packaged libraries for numerical
computation and visualization.

Using Python software for visualizing/regridding/analyzing GEOS-Chem
output also facilitates using GEOS-Chem in cloud-computing environments,
such as the Amazon EC2 platform.

## Using Python

Lizzie Lundgren set up a shared python environment for the Jacob group
on Cannon. The Jacob group python environment was created using:

\<code\> \> wget
<https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh>
-O miniconda.sh \> bash miniconda.sh -b -p
/n/jacob\_lab/Lab/python/miniconda \> export
PATH="/n/jacob\_lab/Lab/python/miniconda/bin:$PATH" \> conda env create
--file jacob.yml\</code\>

The environment file (`jacob.yml`) contains the package list to install
beyond what comes with miniconda. It is based on the recommended
environment for use with GCPy, the python package for GEOS-Chem
benchmarking.

To make the environment available, add the path to your `PATH`
variables. There are a couple options including:

1\. Use this version of python (3.6.2) by default but manually activate
the jacob environment when you want to use it and deactivate when you
are done (**recommended**):

``` 
 > export PATH=/n/jacob_lab/Lab/python/miniconda/bin:$PATH
```

  - To activate this environment: `source activate jacob`
  - To view the list of installed packages: `conda list`
  - To deactivate an active environment: `source deactivate`

2\. Do not use use this python version or the jacob environment by
default. You should do this if you want to use python2 by default,
although this is not recommended.

``` 
 > export PATH=$PATH:/n/jacob_lab/Lab/python/miniconda/bin
```

3\. You may create one or more of your own python environments by
following the above steps in your home directory. Simply customize your
own YAML (`.yml`) file with the packages you would like to install and
the environment name.

See also:

  - <https://medium.com/@rabernat/custom-conda-environments-for-data-science-on-hpc-clusters-32d58c63aa95>
  - <https://conda.io/miniconda.html>
  - <http://yaml.org/>

## Setting up Jupyter notebook using port forwarding

To open a Jupyter notebook, you must first log into the VDI (virtual
desktop interface. Please see these links for more information:

1.  [Log into
    VDI](https://www.rc.fas.harvard.edu/resources/documentation/virtual-desktop)
2.  [Running Jupyter notebook on
    VDI](https://www.rc.fas.harvard.edu/resources/documentation/vdi-apps/#Jupyter_Notebook)

\--- *GEOS-Chem Support Team 2019/09/16 19:02*

## Using IDL

IDL can be used in conjunction with the [GAMAP software
package](http://acmg.seas.harvard.edu/gamap/doc/) to plot GEOS-Chem
model output. You must have the following line in your `~/.bashrc` file:

``` 
 module load IDL                                            # Load IDL
```

This line is already included in the default `~/.bashrc` file that you
obtain from our [repository of startup
scripts](/wiki/as/startup_scripts).

Odyssey has several other software packages (e.g. R, Python) available
for analyzing and plotting output. For more information, see the
[software on Odyssey
page](https://rc.fas.harvard.edu/resources/documentation/software-on-odyssey/).

## View PS and PDF files

Use the Evince document viewer available on Odyssey to view PostScript
and PDF files. Type the following command:

``` 
 evince FILENAME
```

You must have X11 forwarding enabled for Evince to work properly.

## Mount Cannon directories on your desktop

You can mount Cannon file systems, like your home directory or lab
shares, on your desktop to simplify data transfer and allow for viewing
files directly on your computer. Complete instructions can be found on
RC's [access and login
page](https://rc.fas.harvard.edu/resources/access-and-login/#Mounting_Odyssey_storage_like_home_directories_and_lab_shares_on_your_desktop).

\--- *[Melissa Sulprizio](mpayer@seas.harvard.edu) 2015/08/10 14:56*

## Return to Menu

  - [Go to Harvard Atmospheric Sciences Wiki Home](/start)
  - [Go to "The Basics" Menu](/wiki/basics)
