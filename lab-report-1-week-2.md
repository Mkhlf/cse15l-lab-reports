### Lab Report 1
# Remote Access



### Table of content:

  - [Installing VScode](#installing-vscode)
  - [Remotely Connecting](#remotely-connecting)
  - [Trying Some Commands](#trying-some-commands)
  - [Moving Files with `scp`](#moving-files-with-scp)
  - [Setting an SSH Key](#setting-an-ssh-key)
  - [Optimizing Remote Running](#optimizing-remote-running)

---

## Installing VScode:

To install VScode, start by visiting [https://code.visualstudio.com/download](https://code.visualstudio.com/download), then download the appropriate version. When you open the app, you should see something similar to this.

![image](lab-report-1-pics/vscode.png)

## Remotely Connecting:

Start by opening the terminal, either in VScode or your own command line. 
![command line](commandLine.png) 
Then, type `$ ssh A` and replace **A** with your address, e.g., cs15lwi22zz@ieng6.ucsd.edu.

If this was your frist time to connect, you might see this massage: 

```
The authenticity of host 'ieng6.ucsd.edu (128.54.70.227)' can't be established.
RSA key fingerprint is SHA256:ksruYwhnYH+sySHnHAtLUHngrPEyZTDl/1x99wUQcec.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Enter yes, then it will asks you about the password.

```
Password: 
```

Enter your password, and you should see something like this:

![ssh1](lab-report-1-pics/ssh1.png) 

## Trying Some Commands:

Here is a list of some helpful command to use:

1. `cd A` helps you change your directory to **A**.
2. `ls` shows all the file in this directory.
3. `touch A` if the file **A** exists it updates the update/modified date of the file and if the file doesn't exist it will create it for you\!  
4. `cp A B` copies files from **A** to **B**.
5. CTRL+D or `exit` to exit the server you're connected to.

![commandLine](lab-report-1-pics/commandLine2.png)

## Moving Files with `scp`:

We will be using `scp` command to copy files from the client, i.e., your desktop, to the server.

Here is an example of how to use it:

![scp](lab-report-1-pics/scp.png)

## Setting an SSH Key:

This step will help you avoid typing down the passowrd when running ```ssh `or` scp```.
Start by running `ssh-keygen` on your device, which will creates two files *public key*(`id_rsa.pub`) and *privte key*(`id_rsa`) stored in `.ssh`. 

![sshKey](lab-report-1-pics/sshkey1.png)

Lastly, make `.ssh` file in your server (using `mkdir` command) and copy the public key to it.

## Optimizing Remote Running:

To make running commands on the server a bit easier and faster you can do the following:

```
$ssh cs15lwi22zz@ieng6.ucsd.edu "cd"
```

Also you can do multiple commands at the same time by using `;`

```
$ssh cs15lwi22zz@ieng6.ucsd.edu "javac WhereAmI.java; java WhereAmI"
```
![runningCommandsEasier](lab-report-1-pics/runningCommandsEasier.png)

By doing so you can save some time when you want to run your code again, as you can use the up-arrow to find that line then re-run it. For instance, it took me about 45 keystrokes to remotely access the server, compile, and run "WhereAmI.java"; however, when I used the up-arrow method, mentioned above, it took 1 keystroke to compile and run the same file from the server. Moreover you can use the same method to copy the file from your local machine to the server and when ever you made changes to the local cpoy you can use the up-arrow to copy the file to server, then use the up-arrow again to compile and run, which would take 3 - 5 keystrokes!

---


Mohammad Alkhalifah Lab's for [CSE15 lab](https://ucsd-cse15l-w22.github.io/)

[Home page](index.md)
 

