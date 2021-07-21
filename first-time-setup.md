# First-time setup for new Harvard-ACMG members

The main computer system that we utilize is **Cannon**, a 60K+ core computer cluster supported by the FAS Division of Science, Research Computing Group at Harvard University.

This page describes how to get set up on Cannon.  Also please be sure to check out the FASRC documentation page (https://docs.rc.fas.harvard.edu), including the [Cannon Cluster Quick Start Guide](https://docs.rc.fas.harvard.edu/kb/quickstart-guide/).

## Apply for computer accounts

  1.  Apply for a Harvard ID at Holyoke Center. This may take a couple of days to process.  You will need an ID number to set up your email and Cannon accounts.

  2. Claim your Harvard Key. See <http://reference.iam.harvard.edu/>.

  3. Get the DUO smartphone app for two-step verification. See <http://huit.harvard.edu/twostep>.

  4. Connect to the Harvard Secure network following <https://getonline.harvard.edu>.

  5. Request an account with Research Computing at <https://portal.rc.fas.harvard.edu/request/account/new>

    - Fill in your name and contact information
    - Pick a username
    - User type should be "Internal User"
    - The department should be "Jacob Group"
    - Pick a password

  6. Once you have a Research Computing account, you will need to [set up OpenAuth for two-factor authentication](https://docs.rc.fas.harvard.edu/kb/openauth/).  If you have a smartphone, we recommend using the DUO app (downloaded in a previous step) or downloading the Google Authenticator app and using that to generate your verification codes.

# Install the Harvard University VPN for remote access

We recommend that you use the Harvard VPN to access Harvard computational resources from off-campus. For more information, please see <https://huit.harvard.edu/vpn>.

## Log into Cannon

### Access Cannon from a PC or Mac

Before you can log into Cannon you must make sure that your PC or Mac has the following:

  1. A terminal application
  2. An X11 application

These are described in more detail on the [Command line access using a terminal](https://docs.rc.fas.harvard.edu/kb/terminal-access/)

For MacOS: we recommend [XQuartz](https://www.xquartz.org/), which provides X11 server and terminal functionality.  MacOS also comes with its own terminal applications (terminal, iTerm).

For PC: We recommend [MobaXterm](https://mobaxterm.mobatek.net/), which provides both the terminal and the X11 server functionality.

Windows 11 (coming soon) will provide some built-in Linux support with the Windows Subsystem for Linux.

### Access Cannon from a Linux-based machine:

To access Cannon from another Linux-based machine, open a terminal window (xterm is best) and type:

  ssh YOUR-USERNAME@login.rc.fas.harvard.edu

You will be prompted to type your password and two-factor code.  For more information, [please visit this page on the FASRC documentation site](https://docs.rc.fas.harvard.edu/kb/terminal-access/#Connecting_via_SSH)

## Copy Cannon startup scripts

[Follow these instructions](cannon-environment-config.md) to copy a set of standard startup files to your own home directory on Cannon. You can modify these files to customize your Unix environment.

## Set up printer drivers on your PC or Mac

  1.  Check the current [list of SEAS network printers](https://www.seas.harvard.edu/office-computing/printing).  We mostly use:
    - `jacobsgroup-prn`: The Jacob-group printer, in the copier room (Pierce 107G)
    - `prc-1flr-clr2`: Color printer, 1st floor pierce
  2.  [Follow these instructions to install the drivers for the SEAS printers](https://www.seas.harvard.edu/computing-office/printing)

## Set up your personal website

See the SEAS Office of Computing [web services page](http://www.seas.harvard.edu/computing-office/web-services) for
more information on creating a personal website.

## Sign up for email lists

See our [Mailing lists wiki page](email-lists.md) for more information how to sign up for the Jacob Group and GEOS-Chem email lists.