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

Note: No characters will appear on the screen when entering the password.

After entering the correct password you should see the following screen:

![loginConfirm](/imgs/loginConfirmation.png)

Note: For first time users, you will be prompted to trust ieng6. Enter ‘yes’ and continue.

#### <p>5) Testing Commands

After logging in you are now ready to use commands. Some important commands are listed below:

> > -cd (directory or ‘..’) - enter the directory or move one directory out if ‘..’ <br/>
> > -ls - list the current files in the directory <br/>
> > -pwd - prints full name (path) of directory you are currently in <br/>
> > -mkdir (directory) - makes a new directory in the directory you are in <br/>
> > -cp (file) (directory) - copies a file and puts copy in the directory you entered <br/>
> > -cat (file) - shows contents of file <br/>

#### 4) Moving files (scp)</p>

One important feature is that you are able to move files from your client computer to the remote server. We can do this by using the key term ‘scp.’ ‘scp’ will always be run from the client.

![scpCmd](/imgs/scpCmd.png)

In this case, I am trying to move a file named “WhereAmI.java” to my remote server, “cs15lfa22gd@ieng6.ucsd.edu”

Now login to the server and you should see the file in your directory by using the command ‘ls.’

![lsCmd](/imgs/lscmd.png)

#### 5) Simplifying Login Process (SSH Key)

You may have noticed by now that it is very annoying to continuously enter you password every time you login to the remote server. In order to combat this issue, we can have the computer remember our password through ssh keys. Enter the following command:

$ ssh-keygen

![sshkeygen](/imgs/sshkeygen.png)

When prompted with the path to save the key, hit enter to save it in the default path. Hit enter again when prompted with a passphrase. You should see the following text indicating that your private and public key have been saved:

Your identification has been saved in /Users/arvin/.ssh/id_rsa.
Your public key has been saved in /Users/arvin/.ssh/id_rsa.pub.

The last step is to copy the public key (NOT private) into the .ssh directory of your remote server. To do this, login to your remote server using ssh and enter the following code to make a directory:

$ mkdir .ssh

Exit the server and use scp back on your client to copy over the public key into the .ssh directory on the server.

$ scp /Users/arvin/.ssh/id_rsa.pub cs15lfa22gd@ieng6.ucsd.edu:~/.ssh/authorized_keys

Note: Make sure to replace the two characters after ‘cs15lfa22’ with your unique characters

Now you should be able to login without entering the password.

### 6) Remote Running Tricks

#### Commands on Client:

When running small commands such as ‘ls’ or ‘pwd,’ you can run these commands on the client using the following format:

$ ssh cs15lfa22gd@ieng6.ucsd.edu "(command)"

For example, you can get the files in the directory of the server you can use:

$ ssh cs15lfa22gd@ieng6.ucsd.edu "ls"

#### Multiple Commands:

In order to enter multiple commands in one line, you can use semicolons:

$ cp WhereAmI.java OtherMain.java; javac OtherMain.java; java WhereAmI
