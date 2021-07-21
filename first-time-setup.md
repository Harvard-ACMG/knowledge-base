# First-time setup instructions for new members of the Jacob group

There are two main systems that we utilize:

1.  **Cannon** (formerly Odyssey): A 60K+ core computer cluster
    supported by the FAS Division of Science, Research Computing Group
    at Harvard University.
      - We recommend that you primarily use the FAS Research Computing
        Cannon cluster.
2.  **AS**: A local computer cluster specifically set up for the Jacob
    Group and maintained by Judit Flo-Gaya.
      - These systems have limited functionality and will likely be
        retired soon.

This page describes how to get set up on Cannon.

## Step 1: Apply for computer accounts

1.  Apply for a Harvard ID at Holyoke Center. This may take a couple of
    days to process.
      - You will need the ID \# to set up your Research Computing and
        `seas.harvard.edu` accounts.
2.  Claim your Harvard Key.
      - See <http://reference.iam.harvard.edu/> 
3.  Get the DUO smartphone app for two-step verification
      - See <http://huit.harvard.edu/twostep> 
4.  Connect to the Harvard Secure network following
    <https://getonline.harvard.edu>
5.  Request an account with Research Computing at
    <https://portal.rc.fas.harvard.edu/request/account/new>
      - Fill in your name and contact information
      - Pick a username
      - User type should be "Internal User"
      - The department should be "Jacob Group"
      - Pick a password
6.  Once you have a Research Computing account, you will need to [set up
    OpenAuth for two-factor
    authentication](https://rc.fas.harvard.edu/resources/access-and-login/#Odyssey_access_requires_the_OpenAuth_tool_for_two_factor_authentication).
    If you have a smartphone, we recommend using the DUO app (downloaded
    in a previous step) or downloading the Google Authenticator app and
    using that to generate your verification codes. 

## Step 2: Log into Cannon (formerly Odyssey)

### Overview of Cannon

Cannon can be accessed by connecting to
`YOUR_USERNAME@login.rc.fas.harvard.edu` or
`YOUR_USERNAME@Cannon.rc.fas.harvard.edu`. For more information see the
[Cluster Quick Start
Guide](https://rc.fas.harvard.edu/resources/odyssey-quickstart-guide/)

### Log into your Unix environment from a PC

Download MobaXterm and use that to connect to Cannon from your PC. We
recommend using MobaXterm instead of PuTTY and XMing. You can download
the free version of MobaXterm from this page:
<https://mobaxterm.mobatek.net/download-home-edition.html>. Click on the
button to download the “Portable edition”. This will download a zip file
with the MobaTerm executable to your PC. You can move the folder to a
convenient location in your “My Documents” path and create a desktop
shortcut. If you previously used PuTTY, MobaXterm will import any
existing PuTTY sessions that you had defined.

### Log into your Unix environment from a Mac

1.  Open an terminal window with **Applications —\> Utilities —\>
    Terminal**
2.  Connect to Cannon by typing `ssh -Y -A YOUR_USERNAME@HOST` at the
    prompt
      - `YOUR_USERNAME` is your Cannon username
      - `HOST` can be one of the following
          - `login.rc.fas.harvard.edu` (**preferred**)
          - `cannon.rc.fas.harvard.edu`

### Copy startup files

[Follow these instructions](/wiki/as/startup_scripts) to copy a set of
standard startup files to your own disk space. You can modify these
files to customize your Unix environment.

## Step 4: Printer setup

### Print from your PC or Mac

1.  Check the current [list of SEAS network
    printers](http://www.seas.harvard.edu/computing-office/printing/public-network-printers).
    We mostly use:
      - `jacobsgroup-prn`: The Jacob-group printer, in the copier room
        (Pierce 107G)
      - `prc-1flr-clr2`: Color printer, 1st floor pierce
2.  [Follow these instructions to install the drivers for the SEAS
    printers](https://www.seas.harvard.edu/computing-office/printing)

### Print from the Unix machines

1.  [Follow these instructions to print from the Unix
    machines](/wiki/as/printers/unix)

## Step 5: Set up your personal website

See the SEAS Office of Computing [web services
page](http://www.seas.harvard.edu/computing-office/web-services) for
more information on creating a personal website.

## Step 6: Sign up for email lists

See our [Mailing lists wiki page](/wiki/as/mailing_lists) for more
information how to sign up for the Jacob Group and GEOS-Chem email
lists.

## Step 7: Get started with GEOS-Chem

1.  [Read the GEOS-Chem
    manual](http://wiki.geos-chem.org/Getting_Started_with_GEOS-Chem)
2.  Take a moment to familiarize yourself with the [GEOS-Chem
    wiki](http://wiki.seas.harvard.edu/geos-chem/index.php/Main_Page)
3.  Take a moment to read through the [GEOS-Chem Style
    Guide](http://acmg.seas.harvard.edu/geos/doc/man/appendix_7.html)

## Step 8: Install the Harvard University VPN for remote access

If you wish to log into Odyssey from off-campus, it is recommended that
you establish a VPN connection. [Follow these instructions to install
and set up the VPN
software](http://wiki.as.harvard.edu/wiki/doku.php/wiki:as:firewall#as-fas_vpn).

## Return to Menu

  - [Go to Harvard Atmospheric Sciences Wiki Home](/start)
  - [Go to "The Basics" Menu](/wiki/basics)
  - [Go to Network Resources Menu](/wiki/computer_and_network_info)
