# A Guide To Course Specific SSH (CSE 15L)

Arvin Zhang

### 1) Obtaining Hostname and Password

In order to login to your remote access computer, two important pieces of information are required initially: hostname and password.

#### Obtaining Hostname:

You can obtain the hostname by looking it up on:<br/>
https://sdacs.ucsd.edu/~icc/index.php

![login screenshot](/imgs/login.png)

Enter your ucsd username as well as your PID and click “submit.”

Upon logging in, you should see the following section (buttons may vary based on current courses).

![additionalAccountSS](/imgs/additionalAccount.png)

The string of characters starting with “cs15lfa22..” is your unique hostname. In combining this string with “@ieng6.ucsd.edu,” you get your complete hostname. For example, from the screenshots above, the hostname would be “cs15lfa22gd@ieng6.ucsd.edu”

#### Obtaining Password:

In order to set a password for the hostname, you need to follow the resetting password process.

### 2) Downloading Necessary Software (Visual Studios)

In order to login to your remote access computer, two things are required initially. Attached below is a separate instruction sheet on how to reset your password.

[How to Reset your Password] (https://docs.google.com/document/d/1hs7CyQeh-MdUfM9uv99i8tqfneos6Y8bDU0uhn1wqho/edit)

After resetting your password, it can take 15 - 60 minutes for the password to be updated. Please be patient.

### 3) Downloading Necessary Software (Visual Studios and Open SSH)

Two softwares recommended are Visual Studios and Open SSH. Visual studios allows easy access for editing code. Open SSH is required and is the basis of remote access.

#### Installing Visual Studios:

You can install visual studio by following the steps in the following link:

https://code.visualstudio.com/

Upon installation you should be met with the following screen:

![vsCode](/imgs/vscode.png)

Note: Small bits of interface may be different depending on your settings (Ex. Background and text color).

#### Installing Open SSH:

As the basis of remote access, Open SSH is essential. In the following link, check if your computer has OpenSSH installed and if not, install it.

[Check and Install OpenSSH] (https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui)

### 4) Login in

To begin, open up visual studios. In order to enter commands, we need to open the terminal panel. At the top of the screen navigate Terminal -> New Terminal.

#### Remote Connecting Command:

$ ssh cs15lfa22zz@ieng6.ucsd.edu

![sshCmd](/imgs/sshCmd.png)

Note: Make sure the ‘zz’ is replaced with your unique hostname that you obtained earlier.
Password:
Upon entering the command, you will be prompted with a password prompt. Enter your password and hit enter.

![passwordCmd](/imgs/passwordcmd.png)
