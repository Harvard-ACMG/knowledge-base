# System Startup Scripts

**NOTE: The Odyssey cluster was renamed to Cannon in September 2019.**

Shell and other startup scripts allow you to customize your environment.
Files like `.cshrc`, `.bashrc`, `.login`, etc. are sourced when you
start a new shell. The files are editable and you can add your own
customizations, transfer settings you used at other institutions, or
settings developed by someone at Harvard. Our systems run a standard
version of Linux (CentOS-5), and default shell settings (in the absence
of customized startup scripts) provide a standard Linux environment.
However, you might benefit from adopting shortcuts developed locally. Be
aware that local customizations might not work at another institution
when you leave our environment.

## Startup scripts for your Unix login environment

### Should I use csh or bash?

While we have traditionally recommended new Jacob Group members to adopt
the C-shell (csh) as their Unix shell, we now recommend that users adopt
the bash shell. This enables us to maintain a common set of startup
files for both the Atmospheric Sciences cluster as well as the Cannon
cluster.

\--- *[Bob Yantosca](yantosca@seas.harvard.edu) 2014/10/30 19:19*

### Repository of startup scripts

The GEOS-Chem Support Team maintains a Git repository of environment
files that you can use to customize your Unix login environment. These
files contain library and other settings specific to the environment you
are working in (Cannon or AS). We highly recommend using them even if
you are already set up on Cannon. In addition, we recommend using them
in your AS environment as well to take advantage of built-in shortcuts
for transferring files between AS and Cannon.

To download this repository, called `env`, to your home directory, type
the following command:

    cd ~
    git clone https://bitbucket.org/gcst/env

Once you have downloaded this repository, you can get the latest updates
at any time:

    cd ~/env
    git pull origin master

To see the contents of the `env` directory, type the following in `env`
or any of its subdirectories:

    ls -a

To get documentation for the files within the `env` directory, navigate
to the `doc` subdirectory and type:

    make doc

The table below lists the files in the `env` repository and the
environments where each file is relevant (Cannon, AS, or both):

| File                                          | Environment     | Description                                                                                                                                                                                                                                                                                                                               |
| --------------------------------------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `env/root/.bash_profile`                      | Both            | Main startup file for the `bash` shell. It calls the `.bashrc` file to apply your user-defined settings.                                                                                                                                                                                                                                  |
| `env/root/.bashrc_AS`                         | **AS only**     | User-defined settings for the interactive non-login bash shell in your AS environment. It is applied each time you open a new window or session. Note that you must rename it `.bashrc` when copying to your home directory on AS.                                                                                                        |
| `env/root/.bashrc_OD`                         | **Cannon only** | User-defined settings for the interactive non-login bash shell in your Cannon environment. It is applied each time you open a new window or session. Note that you must rename it `.bashrc` when copying to your home directory on Cannon.                                                                                                |
| `env/root/.login`                             | **AS only**     | `csh/tcsh` startup file for login shell. This file is only read at startup.                                                                                                                                                                                                                                                               |
| `env/root/.cshrc`                             | **AS only**     | User-defined settings for the interactive non-login `csh` or `tcsh` shells. It is applied each time you open a new window or session.                                                                                                                                                                                                     |
| `env/root/.my_personal_settings`              | Both            | Where to place your own aliases and settings to customize your login environment. This file is blank when you download it (except for a documentation header) and gets called by the `.bashrc` file. Note that the .bashrc files come with some pre-defined aliases worth checking out.                                                   |
| `env/root/.emacs`                             | Both            | Various customizations for the Emacs editor (font sizes, colors, key bindings, etc.)                                                                                                                                                                                                                                                      |
| `env/root/.Xdefaults`                         | Both            | Settings (fonts, colors) for Xterminal windows applied each time you type `xterm`.                                                                                                                                                                                                                                                        |
| `env/init/init.gc-classic.gfortran71.centos7` | **Cannon only** | Loads modules (and sets environment variables) for using GEOS-Chem "Classic" with gfortran 7.1.0 and CentOS7.                                                                                                                                                                                                                             |
| `env/init/init.gc-classic.ifort17.centos7`    | **Cannon only** | Loads modules (and sets environment variables) for using GEOS-Chem "Classic" with ifort 17.0.4.and CentOS7.                                                                                                                                                                                                                               |
| `env/init/init.gc-classic.ifort15.centos7`    | **Cannon only** | Loads modules (and sets environment variables) for using GEOS-Chem "Classic" with ifort 15.0.0 and CentOS7.                                                                                                                                                                                                                               |
| `env/init/init.gc-classic.ifort11.centos7`    | **Cannon only** | Loads modules (and sets environment variables) for using GEOS-Chem "Classic" with ifort 11.1.069 and CentOS7.                                                                                                                                                                                                                             |
| `env/bin/xt`                                  | Both            | Perl script that starts up an Xwindow and logs into Unix machine via OpenSSH                                                                                                                                                                                                                                                              |
| `env/bin/startup`                             | Both            | Utility shell script to open 1 Emacs editory and 4 xterm windows.                                                                                                                                                                                                                                                                         |
| `env/bin/interactive`                         | Both            | Shell script to request an [interactive SLURM session](/wiki/as/slurm#Using_the_interactive_openmp_script).                                                                                                                                                                                                                               |
| `env/bin/interactive_openmp`                  | Both            | Shell script to request an [interactive SLURM session](/wiki/as/slurm#Using_the_interactive_openmp_script), geared for OpenMP jobs (i.e. all CPUs on the same node).                                                                                                                                                                      |
| `env/bin/awake.sh`                            | Both            | Shell script to print a character to the screen every 10 minutes. This will [prevent your interactive SLURM session (started with `interactive` or `interactive_openmp`) from freezing up](/wiki/as/slurm#problem_with_Cannon_interactive_sessions_freezing_up). (This is an issue that RC has yet to fix with the interactive sessions.) |
| `env/bin/jobinfo`                             | Both            | Perl script that [displays information about a SLURM job](/wiki/as/slurm#the_jobinfo_script) (e.g. memory used, which nodes the job ran on, etc.). This displays the output from `sacct -l -j` in a more easy-to-read format.                                                                                                             |
| `env/bin/ncl.el`                              | Both            | Emacs file to colorized NCAR Command Language code (NCL)                                                                                                                                                                                                                                                                                  |
| `env/bin/isCoards`                            | Both            | Convenience script to determine if a netCDF file is COARDS-compliant or not. This will help you [prepare netCDF data files that can be read by HEMCO](http://wiki.seas.harvard.edu/geos-chem/index.php/Preparing_data_files_for_use_with_HEMCO).                                                                                          |
| `env/.ssh/config`                             | **AS only**     | Text file defining a host called `OD` with `ControlMaster` turned on. The `ControlMaster` option allows you to connect to Cannon once and any other subsequent ssh sessions (`scp`, `rsync`, etc.) will not require re-authentication.                                                                                                    |
| `env/IDL/idl_startup.pro`                     | Both            | Default settings for IDL. It also tells IDL where to find the GAMAP routines. You can tell IDL where to find your own personal IDL routines by adding another `Expand_Path` line to `IDL_PATH`.                                                                                                                                           |
| `env/bash/run_geos_chem`                      | **Cannon only** | Script to [submit a GEOS-Chem job to SLURM](/wiki/as/slurm#submit_jobs_to_the_queue). You will need to copy this script to your run directory and modify the `SBATCH` parameters for your simulation.                                                                                                                                     |
| `env/bash/job_depend.pl`                      | **Cannon only** | Script to [submit several GEOS-Chem jobs to SLURM with job dependencies](/wiki/as/slurm#specify_job_dependencies). In other words, so that each GEOS-Chem job will not start until the previous job has finished.                                                                                                                         |
| `env/doc/Makefile`                            | Both            | Makefile to create documentation for the `env` directory.                                                                                                                                                                                                                                                                                 |
| `env/doc/protex`                              | Both            | Perl script to convert `env` file Protex headers into a `.tex` file.                                                                                                                                                                                                                                                                      |
| `env/doc/intro.txt`                           | Both            | Documentation header information.                                                                                                                                                                                                                                                                                                         |
| `env/doc/hangcaption.sty`                     | Both            | File needed for generating LaTeX documentation.                                                                                                                                                                                                                                                                                           |

\--- *GEOS-Chem Support Team 2018/07/16 17:21*

### Setting Up Your Environment

After you clone the git repository, you will need to copy the files in
the ["env" repository of startup
scripts](/wiki/as/startup_scripts#repository_of_startup_scripts) to the
appropriate locations. We have set up a script called `copy_env` to do
this for you. To distribute the files, type the following:

    cd ~/env
    ./copy_env

Next, you will need to modify the following files that were just copied.
Open each of the following files in a text editor (e.g. `emacs
FILENAME`) and modify.

**1. `~/.emacs`:**

  - Change `format-time-string` to include your signature. For example:

<!-- end list -->

``` 
   ;; To insert a basic time stamp in a buffer
   (defun insert-timestamp ()
     "Insert a nicely formated date string."
     (interactive)
     (insert (format-time-string "!  %e %b %Y - {YOUR_NAME}- ")))
```

  - Change `user-mail-address` to your email address. For example:

<!-- end list -->

``` 
   '(user-mail-address "{YOUR_EMAIL_ADDRESS}"))
```

\*\*2. `~/.ssh/config`: (AS only) \*\*

  - Change `User` to reflect your Research Computing username. For
    example:

<!-- end list -->

``` 
   Host OD
       User {YOUR_USERNAME}
       HostName login.rc.fas.harvard.edu
       ControlMaster auto
       ControlPath ~/.ssh/%r@%h:%p
```

**3. `~/bash/run_geos_chem`: (Odyssey only)**

  - This script can be renamed and copied to each of your run
    directories
  - Modify the other `#SBATCH` lines to set key parameters for the SLURM
    scheduler. See the script's ProTeX header or RC's [running jobs
    page](https://rc.fas.harvard.edu/resources/running-jobs/#Submitting_batch_jobs_using_the_sbatch_command)
    for more information about these options.
  - NOTE: The `SBATCH` parameters in the example script are configured
    for a 4x5 GEOS-Chem benchmark simulation. You will need to modify
    the time (`-t`) and memory (`--mem-per-cpu==`) options for other
    resolutions and simulation types. See our [request enough time and
    memory wiki post](/[[wiki/as/SLURM#request_enough_time_and_memory)
    for more information.

\*\* 4. Other files \*\*

Feel free to customize your startup settings however you wish. For
example, you may want to adjust your emacs display settings in
`~/.emacs`, add your own settings in `~/.my_personal_settings`, and
customize the look of your xterm windows in `~/.Xdefaults`.

Finally, to make use of all of your new or updated startup files, log
out of your system and log back in again.

## Advanced Options

### Colorized Directory Listing

The dircolors command can be used to set [ANSI VT100 terminal
colors](http://graphcomp.com/info/specs/ansi_col.html#colors) for
different types of files in a directory listing. The output of dircolors
(listed below here) can be then cut & pasted into your .cshrc file to
set the LS\_COLORS environment variable:

#### For bash users

These commands are contained in your `~/.bashrc` file:

    # %%% Settings for colorization %%%
    export GREP_COLOR=32
    export LS_COLORS='no=00:fi=00:di=01;33:ln=01;36:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:su=37;41:sg=30;43:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;37:*.tgz=01;37:*.arj=01;37:*.taz=01;37:*.lzh=01;37:*.zip=01;37:*.z=01;37:*.Z=01;37:*.gz=01;37:*.bz2=01;37:*.deb=01;37:*.rpm=01;37:*.jar=01;37:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.avi=01;35:*.fli=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.flac=01;35:*.mp3=01;35:*.mpc=01;35:*.ogg=01;35:*.wav=01;35:'
    
    alias  grep="grep --color=auto"

NOTE: Newer versions of Linux do not allow the old `GREP_OPTIONS`
variable to be used anymore. Therefore, we can turn on colors in the
`grep` ouptut by defining an alias for `grep` with the `--color=auto`
keyword added.

#### For csh or tcsh users

These commands are contained in your `~/.cshrc` file:

    # Set the colors to my preferences
    # Get the string below by typing "dircolors ~/.dircolors.db"
    setenv LS_COLORS 'no=00:fi=00:di=01;33:ln=01;36:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:su=37;41:sg=30;43:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;37:*.tgz=01;37:*.arj=01;37:*.taz=01;37:*.lzh=01;37:*.zip=01;37:*.z=01;37:*.Z=01;37:*.gz=01;37:*.bz2=01;37:*.deb=01;37:*.rpm=01;37:*.jar=01;37:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.avi=01;35:*.fli=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.flac=01;35:*.mp3=01;35:*.mpc=01;35:*.ogg=01;35:*.wav=01;35:'

Other color options for your .cshrc are:

    # to get list of completions with the tab key, and in color!
    set    autolist
    set    color
    set    colorcat
    
    # grep colorized output (32 is green, list of colors here:
    # http://www.termsys.demon.co.uk/vtansi.htm#colors
    setenv GREP_OPTIONS --color=auto
    setenv GREP_COLOR 32

### Abbreviated Machine Names

Usually, the output of the Unix uname command returns the full name of
the host machine (i.e. `YOUR_LOGIN_NAME.as.harvard.edu`). However, it is
often desirable to obtain the short machine name (i.e. without the
`.as.harvard.edu`) for use in Unix prompts or other contexts.

#### For bash users

Add this line to your `.bashrc` file:

    # %%%%% Set Unix prompt to be "[USER@HOST CWD]%" %%%%%
    PS1="[\u@\h \W]% "

It's just that easy.

#### For csh users

Adding the following line to your `.cshrc` file defines an abbreviated
hostname string.

    set hostabbr  = `uname -n | sed 's/.as.harvard.edu//'`

The Unix sed stream editor is used to remove the ".as.harvard.edu"
string.

Then you can use the `hostabbr` variable when setting the Unix prompt in
your `~/.cshrc`.

    # Set prompt to display directory name 
    set    prompt   =  "[! $hostabbr $cwd:t]% "
    alias  cd  'set old=$cwd; chdir \!*; set prompt = "[! $hostabbr $cwd:t]% "'

## Useful aliases

As described above, you can place your own aliases and settings in your
`~/.my_personal_aliases` file. This file is blank when you download it
(except for a documentation header) and gets called by the `.bashrc`
file. If you prefer, you may add aliases to your `.bashrc` file instead,
but that will complicate updating your `.bashrc` file with the standard
version provided in the env repository when recommended by the GCST.

Below we list some useful aliases that you may want to use. Note that
the `.bashrc` files provided in the env repository also come with some
pre-defined aliases worth checking out. To view a list of all aliases
defined in your environment, type the command `alias`.

| Alias                                                                                                 | Description                                                                                                                                                 |
| ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| General aliases                                                                                       |                                                                                                                                                             |
| `alias c="clear"`                                                                                     | Clear the terminal window                                                                                                                                   |
| `alias diff="colordiff"`                                                                              | Colorize output when comparing two files                                                                                                                    |
| `alias h="history"`                                                                                   | Shortcut for `history`; display previous commands                                                                                                           |
| `alias j="jobs -l"`                                                                                   | Shortcut for `jobs`; list the status of all running jobs.                                                                                                   |
| `alias dirsize="du -ch . \| grep total"`                                                              | Display size of current working directory                                                                                                                   |
| `alias rm="rm -Iv"`                                                                                   | Prompt if deleting more than 3 files at a time and display files that are deleted                                                                           |
| SLURM aliases                                                                                         |                                                                                                                                                             |
| `alias sq="squeue --format='%.8i %.10P %.14j %.10u %.2t %.10M %.6D %.6C %R' --exact-partition-names"` | Format `squeue` output for displaying jobs running on a given partition or jobs running for a given user (Example usage: `sq -p jacob`, `sq -u msulprizio`) |
| `alias si="sinfo --format='%.11P %.20N %.11T %.6D %.4c %C'"`                                          | Format `sinfo` output for displaying information about Slurm nodes and partitions (Example usage: `si -p jacob`)                                            |
| `alias hinfo="sinfo --format='%.11P %.20N %.11T %.6D %.4c %C' -p huce_intel"`                         | Display information about `huce_intel` partition                                                                                                            |
| `alias sinfo="sinfo --format='%.11P %.20N %.11T %.6D %.4c %C' -p shared"`                             | Display information about `shared` partition                                                                                                                |
| `alias tinfo="sinfo --format='%.11P %.20N %.11T %.6D %.4c %C' -p regal"`                              | Display information about `test` partition                                                                                                                  |
| `alias jcheck="lsload \| grep -e 'jacob\\|Hostname'"`                                                 | Display load information on all `holyjacob*` nodes                                                                                                          |
| `alias myfs="sshare -u <Username> -U"`                                                                | Display your [Fair-Share factor and other SLURM share information](https://slurm.schedmd.com/sshare.html) (replace `<Username>` with your username)         |
| `alias jacobfs="sshare -A jacob_lab -a"`                                                              | Display the Fair-Share factor for all users in jacob\_lab                                                                                                   |
| Module load aliases                                                                                   |                                                                                                                                                             |
| `alias py="module load python/3.4.1-fasrc01; module list"`                                            | Load specific version of python and print confirmation to screen                                                                                            |
| Other aliases                                                                                         |                                                                                                                                                             |
| Add your useful aliases here\!                                                                        |                                                                                                                                                             |

## Return to Menu

  - [Go to Harvard Atmospheric Sciences Wiki Home](/start)
  - [Go to "The Basics" Menu](/wiki/basics)
  - [Go to Network Resources Menu](/wiki/computer_and_network_info)

\--- *GEOS-Chem Support Team 2019/09/16 18:42*
