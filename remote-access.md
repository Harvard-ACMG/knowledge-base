# Access from remote sites

Our local network (VLAN104) is protected by a firewall that blocks
connections initiated from other networks. When you are away from the
office, you may use the Harvard University IT (HUIT) VPN to login to the
Atmospheric Sciences systems.

## Preliminary steps

These steps only need to be done once.

### Set up your Harvard Key

If you are new to Harvard, you will need to claim your Harvard Key. This
is the password with which you will login to many of Harvard's IT
assets. Logging into the AS or Odyssey machines via the Virtual Private
Network (VPN) now requires you to have a Harvard Key. For more
information, see this web page: <http://reference.iam.harvard.edu/>

\--- *GEOS-Chem Support Team 2016/10/27 20:31*

### Get the DUO mobile app for two-step authentication

It is now necessary to use two-step authentication whenever you
establish a connection to a Harvard machine via the VPN network. The
easiest way to do this is to install the DUO app on your smartphone as
described on this web page: <http://huit.harvard.edu/twostep>.

Each time you use your Harvard Key, DUO will send a "push" to your
smartphone. To accept the push, click on the checkmark button. This lets
the system know that it is really you who are logging in (and not a
hacker).

We strongly recommend that you also add a landline to DUO, so that the
DUO service can call you with a numerical code. This will allow you to
still login to the VPN even if you forget to bring your smartphone with
you.

\--- *GEOS-Chem Support Team 2016/10/27 20:31*

### Install the HUIT VPN software

The procedure for installing and using the HUIT VPN software is listed
below. We have confirmed that these instructions are correct for both
Mac and PC platforms.

The HUIT VPN is powered by the Cisco AnyConnect Secure Mobility Client.
You only need to do these steps when installing the HUIT VPN for the
first time.

1.  Download the Cisco AnyConnect Secure Mobility Client by visiting
    <https://vpn.harvard.edu>.
2.  Install AnyConnect. Enter the following information when prompted:
    1.  **Username:** Enter your email address followed by \#AS (e.g.
        `myname@seas.harvard.edu#AS`), 
    2.  **Password:** Enter your Harvard Key
    3.  **Two-step verification:** Enter the numerical code from the DUO
        app on your smartphone. You may also leave this field blank,
        which will cause DUO to send you a push.
3.  Follow the directions as prompted. This will begin the AnyConnect
    installation sequence.
4.  For more information, please see the [Cisco AnyConnect
    documentation](https://www.noc.harvard.edu/content/open/documentation/Cisco+AnyConnect+VPN+Client+Installation+Guide.pdf).

**NOTE**: If <https://vpn.harvard.edu> doesn't allow you to connect to
the AS cluster, then you may have to use <https://vpn5.harvard.edu>.

\--- *GEOS-Chem Support Team 2016/10/27 20:31*

## Logging in from a remote site via the VPN

Use these steps to login to either the Atmospheric Sciences cluster or
to the FAS Research Computing cluster.

If you login to the Atmospheric Sciences VPN, you can reach both the AS
and Odyssey clusters. But if you login to the FAS Research Computing
cluster, you will only be able to reach Odyssey.

### Login to the Atmospheric Sciences cluster

Once the Cisco AnyConnect Secure Mobility Client is installed, you can
follow these instructions every time you wish to remotely access the
Atmospheric Sciences login nodes:

1.  Launch the AnyConnect application:
    1.  **Windows:** Click on the "Cisco AnyConnect" folder in your
        Start Menu. Then open the folder and click on the AnyConnect
        application.
    2.  **Mac:** Launch AnyConnect from your Applications folder.
2.  Click on the AnyConnect window.
    1.  Enter `vpn.harvard.edu` in the text box and click `Connect`.
3.  Provide the following information when prompted:
    1.  **Username:** Enter your email address followed by "\#AS" (e.g
        `myname@seas.harvard.edu#AS`)
    2.  **Password:** Enter your Harvard Key
    3.  **Two-step authentication:** Enter the numerical code from the
        DUO app on your smartphone. You may also leave this field blank,
        which will cause DUO to send you a push.
4.  Login to your Atmospheric Sciences account with a terminal window. 
    1.  **Windows:** [Use the PuTTY terminal
        application](/wiki/as/putty) to login to
        `USERNAME@USERNAME.as.harvard.edu`. For more information about
        how to configure PuTTY, [please see these
        directions](/wiki/as/putty_config).
    2.  **Mac:** Login with this command: `ssh -Y
        USERNAME@USERNAME.as.harvard.edu`.

\--- *GEOS-Chem Support Team 2014/10/30 20:21*

### Login to Research Computing clusters

Once you have installed the HUIT VPN, [as described
above](#Installing%20the%20VPN%20software), you can use that to login to
the FAS Research Computing "Cannon" supercomputer
(`cannon.rc.fas.harvard.edu`) from your laptop or home PC.

**NOTE: If you login to the Research Computing VPN, you will not be able
to "see" the Atmospheric Sciences cluster.**

1.  Launch the AnyConnect application:
    1.  **Windows:** Click on the "Cisco AnyConnect" folder in your
        Start Menu. Then open the folder and click on the AnyConnect
        application.
    2.  **Mac:** Launch AnyConnect from your Applications folder.
2.  Click on the AnyConnect window.
    1.  Enter `vpn.rc.fas.harvard.edu` in the text box and click
        `Connect`.
3.  Provide the following information when prompted:
    1.  **Username:** Enter your email (e.g. myname@seas.harvard.edu)
    2.  **Password:** Enter your Harvard Key
    3.  **Two-step authentication:** Enter the numerical code from the
        DUO app on your smartphone. You may also leave this field blank,
        which will cause DUO to send you a push.
4.  Login to your Research Computing account with a terminal window. 
    1.  **Windows:** [Use the PuTTY terminal
        application](/wiki/as/putty) to login to
        `USERNAME@rc.fas.harvard.edu`. For more information about how to
        configure PuTTY, [please see these
        directions](/wiki/as/putty_config).
    2.  **Mac:** Login with this command: `ssh -Y
        USERNAME@login.rc.fas.harvard.edu`.

## Security Practices

If a local security problem originates from a remote network, a remote
machine, or a user working from a remote site, remote access from the
network, the machine, or the user may be blocked without warning until
all issues are resolved, regardless of the access method. It is your
responsibility to learn and implement effective security procedures for
your home network or any other remote site, and for your computer. If
you cannot protect your connection to the local network from a remote
site, do not connect\!

The following references might prove helpful:

  - [Home Computer
    Security](http://www.cert.org/homeusers/HomeComputerSecurity/)
  - [Home Network
    Security](http://www.cert.org/tech_tips/home_networks.html)

## Return to Menu

  - [Go to Harvard Atmospheric Sciences Wiki Home](/start)
  - [Go to "The Basics" Menu](/wiki/basics)
  - [Go to Network Resources Menu](/wiki/computer_and_network_info)
