# Operating system updates on Odyssey

Several important changes were made to Odyssey in May 2018, and again in
June 2019. See below for more information:

## What changed and when did it happen?

### What changed in May 2018?

**CENTOS 7 UPGRADE (ODYSSEY3)**

During the May 2018 power-down maintenance window, FAS Research
computing upgraded all Odyssey partitions to CentOS 7. This was required
as part of the Odyssey 3 update.

This update affects all Odyssey users.

### Why was the operating system updated in May 2018?

From <https://www.rc.fas.harvard.edu/odyssey-3-the-next-generation/>:

The operating system on all login and compute nodes of Odyssey was
updated from CentOS 6.8 to the latest release of CentOS 7.

There are several advantages to the more modern OS:

  - Support to launch modern container at runtime (i.e. Singularity ) 
  - Support for new technologies such as Xeon Phi, GPU's, 3DXpoint
    Memory, etc.
  - Performance and power improvements
  - Security enhancements
  - Support for newer code bases

### What will change in June 2019?

A further Odyssey update will be done on June 12th, 2019. FAS Research
computing wrote:

> SOFTWARE MODULES - \!\!\! IF YOU SUBMIT JOBS, PLEASE READ \!\!\!
> 
> \======================
> 
> After June 12th, 2019, EasyBuild will be added to all user
> environments. This requires your attention as your job scripts may
> fail if module calls do not use the full name.

\>

> For best interoperability of EasyBuild based modules with existing
> software modules, please use complete module names and versions to
> make sure the correct software modules are loaded in your user
> environment.

Therefore, to continue using existing software modules (loaded via the
`module` command), after June 12, 2019, you must now type the full
module name with version number, e.g.

    module load git/2.17.0-fasrc01
    module load perl/5.26.1-fasrc01
    ... etc...

We have made these changes in the text below. We have also updated all
of the init scripts in the [repository of startup
scripts](/wiki/as/startup_scripts) to use full version numbers when
loading modules. Please download these scripts (or make the
corresponding changes in your own startup scripts).

## What do I need to do?

You will need to modify your startup scripts so that the proper modules
will be loaded. Some modules commonly used by GEOS-Chem (like netCDF)
depend on new modules that were created especially for the CentOS7
operating system.

### Steps to follow (only do these once)

**1.** If you haven't done so already, clone our [repository of startup
scripts](/wiki/as/startup_scripts).

    cd ~/env
    git clone https://bitbucket.org/gcst/env

**2.** Pull the latest updates from the [repository of startup
scripts](/wiki/as/startup_scripts):

``` 
cd ~/env
git pull origin master 
```

**3.** If you haven't done so already, create the `init` folder in your
home directory:

    cd ~
    mkdir init

**4.** Copy the following files to the `init` folder:

    cd ~/init
    cp ~/env/init/*.centos7 .
    ls

You should now see the following new files in the `init` folder:

    init.gc-classic.gfortran82.centos7   # Contains commands to load modules for GNU Fortran 8.2.0
    init.gc-classic.gfortran71.centos7   # Contains commands to load modules for GNU Fortran 7.1.0
    init.gc-classic.ifort17.centos7      # Contains commands to load modules for Intel Fortran 17.0.4
    init.gc-classic.ifort15.centos7      # Contains commands to load modules for Intel Fortran 15.0.0
    init.gc-classic.ifort11.centos7      # Contains commands to load modules for Intel Fortran 11.1.069

**5.** Back up your existing `~/.bashrc` file:

    cd ~
    cp ~/.bashrc ~/.bashrc.centos6

**6.** Copy the new `.bashrc` file for use with CentOS7 to your home
directory:

    cp ~/env/root/.bashrc_OD_centos7 ~/.bashrc

**7.** Take a minute to open both the new and old `.bashrc` files in
your favorite editor. Copy any settings that you wish to retain from the
`.bashrc.centos6` backup file to the new `.bashrc` file.

\==== Can I manually make changes to my custom environment scripts? ====

Many of you might have special versions of `~/.bashrc` or other scripts
that have custom commands for special software packages. If you don't
want to throw those out completely, you can just manually add the new
[the new CentOS7 module load
commands](#CentOS7%20module%20combinations%20tested%20with%20GEOS-Chem)
into these existing scripts instead of following the [steps listed in
the previous
section](#Steps%20to%20follow%20\(only%20do%20these%20once\)).

One important caveat: make sure to remove all instances of the following
line:

    source new-modules.sh

This line was specific to the CentOS6 modules and is no longer needed
for CentOS7.

### Logging into Odyssey using these new scripts

Once you have followed the steps [the steps listed in the preceding
section](#Steps%20to%20follow%20\(only%20do%20these%20once\)), you can
then login to Odyssey as you normally would. When you login to a window,
no modules will have been loaded. You can then type one of the following
commands:

    load_gf82   # Loads the proper modules for the gfortran compiler (version 8.2.0)
    
    load_gf71   # Loads the proper modules for the gfortran compiler (version 7.1.0)
    
    load_if17   # Loads the proper modules for the ifort 17.0.4 compiler
    
    load_if15   # Loads the proper modules for the ifort 15.0.0 compiler
    
    load_if11   # Loads the proper modules for the ifort 11.1.069 compiler

A list of loaded modules will then be displayed. Once this happens, you
can proceed to compile and run GEOS-Chem as you used to.

## CentOS7 module combinations tested with GEOS-Chem

The GEOS-Chem Support Team has been able to compile and run GEOS-Chem
with the following CentOS7 module combinations:

### GEOS-Chem "Classic" with gfortran 8.2.0

Updated 19 Nov 2018.

    module purge
    module load git/2.17.0-fasrc01
    module load perl/5.26.1-fasrc01
    module load IDL/8.4.1-fasrc01
    module load flex/2.6.4-fasrc01
    module load gcc/8.2.0-fasrc01
    module load openmpi/3.1.1-fasrc01
    module load netcdf/4.1.3-fasrc02
    module load emacs/26.1-fasrc01
    module list

### GEOS-Chem "Classic" with gfortran 7.1.0

Updated 13 Jul 2018, after RC rebuilt the emacs module.

**NOTE: The netCDF Operators (nco) and Climate Data Operators (cdo)
packages are only compatible with gfortran 7.1.0. Make sure to load
gfortran 7.1.0 (load\_gf71) if you want to use either nco or cdo to
process netCDF data files\!**

    module purge
    module load git/2.17.0-fasrc01
    module load perl/5.26.1-fasrc01
    module load IDL/8.4.1-fasrc01
    module load flex/2.6.4-fasrc01
    module load gcc/7.1.0-fasrc01
    module load openmpi/2.1.0-fasrc02
    module load netcdf/4.3.2-fasrc05
    module load netcdf-fortran/4.4.0-fasrc03
    module load cdo/1.9.4-fasrc02
    module load nco/4.7.4-fasrc01
    module load ncview/2.1.7-fasrc02
    module load emacs/26.1-fasrc01
    module list

### GEOS-Chem "Classic" with ifort 17.0.4

Updated 13 Jul 2018, after RC rebuilt the emacs module

    module purge
    module load git/2.17.0-fasrc01
    module load perl/5.26.1-fasrc01
    module load IDL/8.4.1-fasrc01
    module load flex/2.6.4-fasrc01
    module load intel/17.0.4-fasrc01
    module load openmpi/2.1.0-fasrc02
    module load netcdf/4.3.2-fasrc05
    module load netcdf-fortran/4.4.0-fasrc03
    module load nco/4.7.4-fasrc01
    module load cdo/1.9.4-fasrc02
    module load ncview/2.1.7-fasrc02
    module load totalview/8.8.0.1-fasrc01
    module load emacs/26.1-fasrc01

### GEOS-Chem "Classic" with ifort 15.0.0

Updated 13 Jul 2018, after RC rebuilt the emacs module

    module purge
    module load git/2.17.0-fasrc01
    module load perl/5.26.1-fasrc01
    module load IDL/8.4.1-fasrc01
    module load flex/2.6.4-fasrc01
    module load intel/15.0.0-fasrc01
    module load openmpi/2.1.0-fasrc02
    module load netcdf/4.3.2-fasrc05
    module load netcdf-fortran/4.4.0-fasrc03
    module load emacs/26.1-fasrc01
    module list

### GEOS-Chem "Classic" with ifort 11.1.069

Updated 13 Jul 2018, after RC rebuilt the emacs module

    export MODULEPATH="/n/holylfs/EXTERNAL_REPOS/GEOS-CHEM/modulefiles:$MODULEPATH"
    module purge
    module load git/2.17.0-fasrc01
    module load perl/5.26.1-fasrc01
    module load IDL/8.4.1-fasrc01
    module load flex/2.6.4-fasrc01
    module load intel/11.1
    module load GEOS-Chem-Libraries
    module load totalview/8.8.0.1-fasrc01
    module load emacs/26.1-fasrc01
    module list
    
    ##### YOU MUST ALSO SET THESE ENVIRONMENT VARIABLES MANUALLY #####
    
    # Manually point to GEOS-Chem-Libraries build
    NETCDF_HOME=/n/holylfs/EXTERNAL_REPOS/GEOS-CHEM/opt/GEOS-Chem-Libraries/ifort/nc4
    export NETCDF_INCLUDE=${NETCDF_HOME}/include
    export NETCDF_LIB=${NETCDF_HOME}/lib
    export NETCDF_BIN=${NETCDF_HOME}/bin
    export PATH="$NETCDF_BIN:$PATH"
    
    # GEOS-Chem-Libraries does not set the compiler variables
    # so we must do that manually here (bmy, 1/27/16)
    export FC=ifort
    export CC=icc
    export CXX=icpc

### GCHP with gfortran 8.2.0 and OpenMPI

Update your GCHP environment file to use the following modules. (These
module loads are included in the sample .bashrc file
`environmentFileSamples/gchp.gfortran82_openmpi_odyssey.env`)

    module purge
    module load git/2.17.0-fasrc01
    module load gcc/8.2.0-fasrc01
    module load openmpi/3.1.1-fasrc01
    module load netcdf/4.1.3-fasrc02

### GCHP with gfortran 7.1.0 and OpenMPI

Update your GCHP environment file to use the following modules. (These
module loads are included in the sample .bashrc file
`environmentFileSamples/m gchp.gfortran71_openmpi_odyssey.env`)

    module purge
    module load git/2.17.0-fasrc01
    module load gcc/7.1.0-fasrc01
    module load openmpi/3.1.1-fasrc01
    module load netcdf/4.1.3-fasrc03

### GCHP with ifort 17.0.4 and OpenMPI

Update your GCHP environment file to use the following modules.

    module purge
    module load git/2.17.0-fasrc01
    module load intel/17.0.4-fasrc01
    module load openmpi/3.1.1-fasrc01
    module load netcdf/4.1.3-fasrc03

\--- *GEOS-Chem Support Team 2018/11/19 22:07*

## Resolving problems with the emacs editor

After the update to CentOS7, we noted a couple of issues with the emacs
text editor. Here are the recommended workarounds:

### Switch to MobaXterm to avoid emacs windows from dying

If you have encountered an error such as this when trying to open an
Emacs editor window:

    libGL error: unable to load driver: swrast_dri.so
    libGL error: failed to load driver: swrast
    X protocol error: BadRequest (invalid request code or no such operation) on protocol request 130
    When compiled with GTK, Emacs cannot recover from X disconnects.
    This is a GTK bug: https://bugzilla.gnome.org/show_bug.cgi?id=85715
    For details, see etc/PROBLEMS.
    
    Backtrace:
    emacs[0x4f98c3]
    emacs[0x4deef1]
    emacs[0x4f9903]
    emacs[0x4b339b]
    emacs[0x4b560c]
    emacs[0x4b566d]
    /lib64/libX11.so.6(_XError+0x12b)[0x2af61ee3b07b]
    /lib64/libX11.so.6(+0x420d7)[0x2af61ee380d7]
    /lib64/libX11.so.6(+0x42195)[0x2af61ee38195]
    /lib64/libX11.so.6(_XReply+0x208)[0x2af61ee39088]
    /lib64/libX11.so.6(XQueryPointer+0x8e)[0x2af61ee2f0be]

then we recommed that you download MobaXterm and using that to connect
to Odyssey from your PC (instead of PuTTY and XMing). For some reason,
there is a compatibility issue with Emacs under PuTTY and Xming that
does not happen with MobaXterm. The issue has been reported to FASRC.

You can download the free version of MobaXterm from this page:
<https://mobaxterm.mobatek.net/download-home-edition.html>. Click on the
button to download the "Portable edition". This will download a zip file
with the MobaTerm executable to your PC. You can move the folder to a
convenient location in your "My Documents" path and create a desktop
shortcut.

MobaXterm will import any existing PuTTY sessions that you have defined.
So you should not have to re-define any account settings.

Also make sure to add these lines in your `.emacs` file, which is
located in your root directory. (The double semi-colons `;;` are comment
characters.)

    ;;=============================================================================
    ;; FONTS - customize to look best on your system!
    ;;=============================================================================
    
    ;; %%%%% NOTE: USE THIS FONT WITH MOBAXTERM (bmy, 5/25/18) %%%%%
    (set-face-font 
     'default "-*-DejaVuSansMono-Bold-R-*-*-*-120-*-*-*-*-iso8859-1" )
    
    ;; %%%%% NOTE: USE THIS FONT WITH PUTTY AND XTERM (bmy, 5/25/18 %%%%%
    ;; Lucida Typewriter 14pt bold
    ;;(set-face-font 
    ;; 'default "-*-Lucidatypewriter-Bold-R-*-*-*-140-*-*-*-*-iso8859-1" )

Bob Yantosca has updated the env repository of startup scripts
accordingly. In particular, you should get the new .emacs file.

FASRC may add a fix so that emacs works with PuTTY and Xming, but we
don't know when that will happen.

\--- *[Bob Yantosca](yantosca@seas.harvard.edu) 2018/05/25 17:32*

### Use the new Emacs 26.1 module

The default Emacs (version 24.1) that shipped with CentOS 7 was a broken
build. In particular, the toolbar menu did not display properly.

To fix the issue, RC built a module for Emacs 26.1, which fixes the
toolbar issue. Please make sure to load

    module load emacs/26.1-fasrc01

into your login environment.

Also note: We have modified the default startup script (env/bin/startup)
to load the new emacs module before trying to open an emacs window:

    # Open an EMACS editor
    # NOTE: Make sure to load the emacs module, which currently has a properly-
    # functioning build of emacs.  The default version is broken. (bmy, 7/13/18)
    module load emacs
    emacs &

\--- *GEOS-Chem Support Team 2018/07/13 21:31*

#### Update June 2019

After the June 2019 Odyssey Shutdown and CentOS7 OS patch, a new default
version of emacs (24.3) was installed. This version no longer has the
issue that caused the menu bar not to appear.

Therefore, you no longer strictly need to use the emacs 26.1 module,
because the default emacs isn't broken anymore. But you can continue
using emacs 26.1 if you so wish, it is a newer version with a few nicer
features.

\--- *GEOS-Chem Support Team 2019/06/19 20:53*

## Return to Menu

  - [Go to Harvard Atmospheric Sciences Wiki Home](/start)
  - [Go to "The Basics" Menu](/wiki/basics)
  - [Go to Network Resources Menu](/wiki/computer_and_network_info)
