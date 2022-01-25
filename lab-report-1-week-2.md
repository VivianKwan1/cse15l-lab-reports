# Remote Access
## Table of Contents
* [Installing VSCode](#installing-vscode)
* [Remotely Connecting](#remotely-connecting)
* [Trying Some Commands](#trying-some-commands)
* [Moving Files with `scp`](#moving-files-with-scp)
* [Setting an SSH Key](#setting-an-ssh-key)
* [Optimizing Remote Running](#optimizing-remote-running)


## Installing VSCode
![Image](./lr1im/ss2.png)
* VSCode can be installed [here](https://code.visualstudio.com/download)
* I already had VSCode installed and set up prior to this class, so no additional setup was needed

## Remotely Connecting
* By using `ssh`, we can remotely connect to another computer (the server)
* Since I'm on Windows, I started by [installing OpenSSH](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse)
* To connect to a UCSD computer, I used the following command in my VSCode Terminal: \
        ```
        $ ssh cs15lwi22acy@ieng6.ucsd.edu
        ```
* This is what I see upon connecting with `ssh`:
![Image](https://cdn.discordapp.com/attachments/487122748162834432/931702943265202256/ss3.PNG)
* I use Ctrl+d to disconnect from the remote computer

## Trying Some Commands
* We also tried several commands on both local and remote computers
* `cd`:
    ![Image](https://media.discordapp.net/attachments/487122748162834432/931702943919534200/ss5.PNG)
    * `cd ~`: return to home directory
    * `cd ..`: go back one directory on the current path
    * `cd <directory name>`: go to specified directory
* `ls`: lists files and directories in current directory
    ![Image](https://media.discordapp.net/attachments/487122748162834432/931702943667847198/ss4.PNG)
    * `ls -l`: lists in longer format with additional information
    * `ls -a`: lists all files and directories, including hidden ones
    * `ls <directory>`: lists files and directories in specified directories
        * permissions restrict access to other people's directories
* `cp <source file> <destination file>`: copies the source file into the destination file (overwrites or creates a new file)
    * `cp <file 1> <file 2> <file 3> <directory>`: copies all files into the specified directory with the same name (will overwrite existing files)
* `cat <file>`: prints the contents of the specified file
    ![Image](https://media.discordapp.net/attachments/487122748162834432/931702944133439539/ss6.PNG)
    * works with multiple files and will print them in order
    
## Moving Files with `scp`
![Image](https://media.discordapp.net/attachments/487122748162834432/931702944347332718/ss7.PNG?width=1763&height=836)
* `scp <file> <server>`: copies files and directories from the client to the remote computer
* Also included in this screenshot is the result of running WhereAmI.java on the server, which printed information about the server rather than the client

## Setting an SSH Key
* SSH keys help us connect to the remote computer more efficiently, as it removes the need to type a password each time
* Steps to set up an SSH key:
    * `ssh-keygen`: generates a private and public key
    * enter desired passphrase (or no passphrase)
    ![Image](https://media.discordapp.net/attachments/487122748162834432/935287156048535562/ss11.PNG)
    * As I'm on Windows, I also needed to follow some [additional steps](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement#user-key-generation)
    * Connect to the server (`ssh`)
    * `mkdir .ssh`: creates a directory named `.ssh`
    * Disconnect from the server
    * `scp <location of public key> <server>:~/.ssh/authorized_keys`: copies public key to the server
    * Now we don't need a password to connect to the server!
    \
    \
    ![Image](https://media.discordapp.net/attachments/487122748162834432/931702944615772210/ss8.PNG)


## Optimizing Remote Running
* To make running commands on the server more efficient, we can use:
    * `ssh <server> <command>`: connects to the server, runs the command, then disconnects
    ![Image](https://media.discordapp.net/attachments/487122748162834432/931702944859033630/ss9.PNG)
    * Note: `ssh <server> <command>; <command>` will run the first command on the server and the second on the client. To run multiple on the server, contain the commands in quotes: \
    `ssh <server> '<command>; <command>'`
    ![Image](https://media.discordapp.net/attachments/487122748162834432/931702945215574096/ss10.PNG)
    * Example: By using these commands, we can save time copying the WhereAmI.java file to the remote server, running it, and disconnecting:
        * `scp WhereAmI.java <server>` `ssh <server>`  `javac WhereAmI.java` `java WhereAmI` `logout` 46 + 24 + 20 + 14 + 6 = 110 keystrokes
        * `scp WhereAmI.java <server>` `ssh <server> 'javac WhereAmI.java; java WhereAmI'` 46 + 50 = 96 keystrokes
    * Also, if we already had these commands in history, we could use the up arrow to access these commands, which would further increase the relative speed that we could execute these commands. For the first we would need to use the up arrow four times to get the line, for four lines (and enter four times), whereas for the second we would only have to use the up arrow twice for two lines and enter twice, going from 20 to 6 keystrokes.