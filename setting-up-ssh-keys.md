# Setting up Secure Shell (ssh) keys

## ssh and keys

### Overview

The ssh software (which consists of `ssh`, `scp`, and `sftp`) that we (and almost all Linux systems) use supports authentication by passwords or key pairs. Key pairs have the following advantages over simple passwords:

Using ssh keys is not difficult if you understand the basic concepts and available tools. There is a command to generate an asymmetric key pair. A key pair consists of a private key and public key.

### Private keys

Your private key will only be installed on the machine from which you are trying to connect to the Unix environment (i.e. your PC or Mac).

Private means just that. The private key should never be exposed for any reason and does not need to reside on a public or multi-user system at all. All you really need to work interactively is a private key on your PC or Mac and a public key on another system.

### Public keys

Public keys will be installed on the Unix environnent(s) that you are trying to connect to.**

The public key can be installed anywhere and can even be sent via email if necessary. All you have to do is to cut-n-paste the each public key into the file `/home/.ssh/YOUR_LOGIN_ID/.ssh/authorized_keys` in your Unix environment.

### Assign a passphrase to your private key!

When you generate a public/private key pair, you will be given the opportunity to create a password (sometimes called "passphrase") for the private key. The password encrypts the private key and makes it more difficult to use if it is stolen (e.g. if your machine is stolen or hacked).

When using ssh with such a key, you would normally be prompted for the password, but MacOSX provides a keychain, and the password for the ssh key can be added to your keychain the first time you use it. Once you provide the password for your keychain, which you’re likely to do for other things anyway, you can use the private key without typing the password until you lock the keychain by rebooting or logging out. Linux provides a similar capability.

The public key will permit you to log in using the key pair when it is placed in your `~/.ssh/authorized_keys`. You can copy and paste the contents of `id_rsa.pub` or copy the file itself to a remote system and then copy it to authorized\_keys or append to authorized\_keys using the cat command. Make certain that your copy-paste operation does not
introduce carriage returns into the key the public key entry in authorized\_keys should all be on one line. The authorized\_keys file itself does not need to be read-protected, but the .ssh directory containing it should be protected (`chmod 700 ~/.ssh`). An authorized\_keys file can contain more than one public key, useful if you are connecting to the Unix environment from more than one PC or Mac. 

### Hazards

Placing a private key on a public system increases the risk of a compromise and should be avoided. In particular, if your private key became readable by others for some reason, or if a system containing your private key were ever compromised, your private key could be stolen and used to access any account on which you had installed the
corresponding public key.

Avoid storing private keys anyplace other than your personal system. If you think you need to keep a private key on one of our systems, please consult with jhy about it. There might be a simple and more secure alternative, and a private key on a remote system is rarely needed. 

## Generating public and private keys

### For PC Users

We recommend that you use the [MobaXterm terminal software](https://mobaxterm.mobatek.net/) and the [FileZilla file transfer software](https://filezilla-project.org/Filezilla).

You can use MobaXterm to create your public/private key pair. For detailed instructions, [please watch this video](https://www.youtube.com/watch?v=sVdbJcX2B10).  Also see the [MobaXterm documentation](https://mobaxterm.mobatek.net/documentation.html) page.

### For Linux or Mac users

On Linux and MacOSX systems, the command to generate a key pair is called `ssh-keygen`. It uses RSA encryption by default. `ssh-keygen` can be called as follows:

    ssh-keygen -t rsa -b 2048

The private key will be named `id_rsa` and the public key will be named `id_rsa.pub`. These files will be stored in your `~/.ssh` directory.  `id_rsa` is the private key, and it should be read-protected against anyone but you (`chmod 600 /path/to/id_rsa`), and it should never reside anyplace where it can be read by anyone else. It is yours, exclusively. `id_rsa.pub` contains the public key that is used to give you permission
to log into other systems.

The `-b 2048` sets up 2048-bit encryption, which is more secure than the default (1028-bit) setting.

## Installing public keys: Connecting from your PC or Mac to a Linux machine

By this point you should have done the following:

  1.  Created a private key and stored it in your `~/.ssh` directory on your PC or Mac
  2.  Created a public key 

Now you need to copy the public key to each Linux machine (or cluster) where we'd like to login. Let's assume we want to login to the Cannon cluster, but this 

Log in to Cannon from your terminal application.

    cd ~/.ssh
    emacs authorized_keys

(you can use vim or any other editor instead of emacs if you wish.)

Then cut & paste the contents of your public key (e.g. `id_rsa.pub`) into the authorized\_keys file. Make sure there are no carriage returns between the lines, as the public key should be one long string. 

## SSH Agent and Agent Forwarding

Placing the public key in `~/.ssh/authorized_keys` enables `ssh` login and `scp` to a remote system from the system using the private key (your PC). But if you want to connect from that remote system to a second remote system, you can still use the same private key on your PC by invoking ssh agent.

If your private key has a password attached to it, then you will be prompted to supply the password once. Then as long as the ssh-agent software is open, you will not need to supply the password again.

### For PC users

We recommend that you use the MobaXterm terminal software for connecting from your PC to Cannon. You can use MobaXterm to create your public/private key pair. For more information, please see this video: <https://www.youtube.com/watch?v=sVdbJcX2B10>

### For Linux or Mac Users

To enable the agent when you login to Cannon a MacOSX or Linux system, use the `-A` option to ssh:

    ssh -YA YOUR-USERNAME@login.rc.fas.harvard.edu

Then to log in to a second system from Cannon, type:

    ssh -YA YOUR-OTHER-USERNAME@a.second.machine.elsewhere

The login to the second machine still relies on the private key on your Linux PC or Mac (the agent refers back to the previous link in the chain, so your private key never leaves your PC). If you wanted to enable X11 forwarding you could use the `-X` or `-Y` command in addition to `-A`.

All you need to access any remote Linux system is your public key in `~/.ssh/authorized_keys` on that system. If several systems share a home directory, placing a public key on one will give you access to all.
