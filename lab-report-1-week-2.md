# Lab Report 1:  Remote Access using VScode

## Installing VScode:

To install VScode, start by visiting [https://code.visualstudio.com/download](https://code.visualstudio.com/download), then download the appropriate version. When you open the app, you should see something similar to this.
![image](vscode.png)

## Remotely Connecting

Start by opening the terminal, either in VScode or your own command line. 
![command line](commandLine.png) 
Then, type `$ ssh address` and replace 'address', e.g., cs15lwi22zz@ieng6.ucsd.edu.

If this was your frist time to connect, you might see this massage 
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
![ssh1](ssh1.png) 

## Trying Some Commands

Here is a list of some helpful command to use:

1. `cd A` helps you change your directory to *A*
2. `ls` shows all the file in this directory
3. `touch A` if the file *A* exists it updates the update/modified date of the file and if the file doesn't exist it will create it for you !  
4. `cp A B` copies files from A to B
![commandLine](commandLine2.png)

## Moving Files with `scp`

## Setting an SSH Key

## Optimizing Remote Running