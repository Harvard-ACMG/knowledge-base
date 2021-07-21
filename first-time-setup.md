# First-time setup for new Harvard-ACMG members

The main computer system that we utilize is **Cannon**, a 60K+ core computer cluster supported by the FAS Division of Science, Research Computing Group at Harvard University.

This page describes how to get set up on Cannon.  Also please be sure to check out the FASRC documentation page (https://docs.rc.fas.harvard.edu), including the [Cannon Cluster Quick Start Guide](https://docs.rc.fas.harvard.edu/kb/quickstart-guide/).  The FASRC documentation is comprehensive and updated frequently to keep pace with system upgrades.

## Apply for computer accounts

  1. Apply for a Harvard ID at Holyoke Center. This may take a couple of days to process.  You will need an ID number to set up your email and Cannon accounts.

  2. Claim your Harvard Key. See <http://reference.iam.harvard.edu/>.

  3. Get the DUO smartphone app for two-step verification. See <http://huit.harvard.edu/twostep>.

  4. Connect to the Harvard Secure network following <https://getonline.harvard.edu>.

  5. [Request a Cannon account](https://docs.rc.fas.harvard.edu/kb/how-do-i-get-a-research-computing-account/) with FAS Research Computing (FASRC).

  6. [Set up OpenAuth for two-factor authentication](https://docs.rc.fas.harvard.edu/kb/openauth/) once your Cannon account is approved.  You can use a smartphone app such as DUO, Google Authenticator, or similar to

  7. Familiarize yourself with the Workflow on Cannon:
    a. View the [Cannon Introductory Training video](https://docs.rc.fas.harvard.edu/kb/quickstart-guide/#4_Review_our_introductory_training).  We also recommend taking an Intoduction to Cannon course (either by Zoom or in-person) the next time it is offered.
    b. Review the [Cannon cluster customs and responsibilities](https://docs.rc.fas.harvard.edu/kb/responsibilities/) document.

## Install the Harvard University VPN for remote access

We recommend that you use the Harvard VPN to access Harvard computational resources from off-campus. For more information, please see <https://huit.harvard.edu/vpn>.

## Log into Cannon

Below you will find some important instructions for how to access Cannon remotely.  Please also refer to the [Cannon Cluster Quick Start Guide](https://docs.rc.fas.harvard.edu/kb/quickstart-guide/).

### Access Cannon from a PC or Mac

Before you can log into Cannon you must make sure that your PC or Mac has the following:

  1. A terminal application
  2. An X11 application

Please see the [Command line access using a terminal](https://docs.rc.fas.harvard.edu/kb/terminal-access/) page at the FASRC documentation site for details.

For MacOS: we recommend [XQuartz](https://www.xquartz.org/), which provides X11 server and terminal functionality.  MacOS also comes with its own terminal applications (terminal, iTerm).

For PC: We recommend [MobaXterm](https://mobaxterm.mobatek.net/), which provides both the terminal and the X11 server functionality.

Windows 11 (coming soon) will provide some built-in Linux support with the Windows Subsystem for Linux.

### Access Cannon from a Linux-based machine:

To access Cannon from another Linux-based machine, open a terminal window (xterm is best) and type:
```
ssh YOUR-USERNAME@login.rc.fas.harvard.edu
```
You will be prompted to type your password and two-factor code.  For more information, [please visit this page on the FASRC documentation site](https://docs.rc.fas.harvard.edu/kb/terminal-access/#Connecting_via_SSH)

## Download startup scripts and software environment files

Clone a copy of the [cannon-env](https://github.com/Harvard-ACMG/cannon-env) repository to your home directory.  You can then copy these scripts to your home directory and further modify them.

For more information, please see our chapters on [Startup Scripts](cannon-environment-config.md) and [Environment files](cannon-environment-files.md)

## Set up printer drivers on your PC or Mac

For more information on setting up printer drivers on your PC or Mac, see our [Accessing Local Printers](accessing-local-printers.md) page.

## Set up your personal website

See the SEAS Office of Computing [web services page](https://www.seas.harvard.edu/office-computing/web-services) for instructions on setting up a SEAS-based website where you can display your research.

## Sign up for email lists

See our [Mailing lists wiki page](email-lists.md) for more information how to sign up for the Harvard-ACMG GEOS-Chem email lists.