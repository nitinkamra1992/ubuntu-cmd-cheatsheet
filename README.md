# Cheatsheet of Ubuntu Commands

This repository contains a cheatsheet of commands for Ubuntu/Linux from various sources for quick reference. It also has links to useful tutorials for topics which cannot be summarized in a small cheatsheet.

#### Sources

- [Raspberry Pi's documentation on Linux](https://www.raspberrypi.org/documentation/linux/)
- [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line)


## Basics of a Terminal/Shell

Terminal/Shell is used for typing and executing commands.
- **echo**: Display a line of text, e.g. `echo hello world` or `echo "hello world"`.
- To terminate an active running command, press **ctrl-c**.

#### Finding Help

- Use `man {command}` to display a manual for a command. Try `man man`!
- Manual pages have a short description. Use `apropos {keyword}` to search the descriptions for instances of `keyword`.
- Some commands are not executables, but shell builtins.
  - `man` only helps for executables. For builtins, use `help {command}`.
  - `help -d` gives a list of builtins.
  - Use `type {command}` to find out if a command is an executable or a shell builtin or an alias. Try `type cd`, `type find`, `type ls`.
- `curl cheat.sh/{command}` gives a cheat sheet with common example usages of `command`.
- If you don't understand a command, can use [explainshell](https://explainshell.com/) to get a helpful breakdown for it.

#### Shell Scripting

- Commands can be combined together in a file which can then be executed.
- Bash is a popularly used language for scripting. Bash files have a `.sh` extension. You must make such a file executable by using `chmod` and then run it by typing `./{filename.sh}`.
- See the [shell scripting tutorial](https://www.shellscript.sh/) for more details.
- The `nano` editor is a simple editor for basic terminal-editing (opening, editing, saving, searching). For power users in a text terminal, learning Vim (vi) is recommended. It is a hard-to-learn but venerable, fast, and full-featured editor.
- TODO: Familiarize yourself with Bash job management: `&`, **ctrl-z**, **ctrl-c**, `jobs`, `fg`, `bg`, `kill`, etc.
- TODO: Learn about file glob expansion with `*` (and perhaps `?` and `[`...`]`) and quoting and the difference between double `"` and single `'` quotes.


## Redirecting Input and Output

- A pipe `|` allows the output from one command to be used as the input for another command. E.g., to only show the first ten entries of the `ls` command, it can be piped through the head command: `ls | head`.
- `>` and `<` can be used to redirect outputs to a stream or a file. `>` overwrites the output file and `>>` appends to it. TODO: Learn about stdout and stderr.


## CPU

- Type `lscpu` for details about your cpu.


## Root User/Superuser (sudo)

- Run commands as the root user by using the `sudo` command:
- Run a superuser shell: `sudo su`.
- Use `sudo -s` for a superuser shell.


## Files and Directories

- List information about the FILEs (default: current directory): `ls [OPTION] [FILE]`. Some useful flags for `ls`: 
    - `-a, --all`: List all files (including hidden files starting with a `.`)
    - `-d, --directory`: List directories themselves, not their contents
    - `-h, --human-readable`: With `-l` and/or `-s`, print human readable sizes
    - `-i, --inode`: Print the index number (inode) of each file
    - `-l`: Long list format to display additional information (permissions, owner, group, size, date and timestamp of last edit) for each file and directory
    - `-R, --recursive`: List subdirectories recursively
    - `-s, --size`: Print the allocated size of each file, in blocks
- Change directory: `cd {path_to_dir}`.
- Present working directory: `pwd`.
- Make directory: `mkdir {dirname}`. Use the `-p` option when creating a path to create parent directories as well.
- Delete file/directory:
    - `rm {filename}`
    - `rm -rf {dirname}`: `-r, -R, --recursive` flag deletes directories and their contents recursively; `-f, --force` never prompts and forcefully deletes even non-empty directories.
- Copy: `cp {src_file} {dst_file}`. Can take `FILE FILE` or `FILE DIR` or `-r DIR DIR`.
- Move/Cut: `mv {src_file} {dst_file}`. Can take `FILE FILE` or `FILE DIR` or `-r DIR DIR`.
- `cat`: List the contents of file or multiple files, e.g. `cat {filename}` or `cat *.txt`.
- `head {filename}`: Displays the beginning of a file. Use `-n` to specify the number of lines to show (default: 10) or `-c` to specify the number of bytes.
- `tail {filename}`: Displays the end of a file. The starting point in the file can be specified either through `-c` for bytes or `-n` for number of lines. `tail -f {filename}` allows viewing growing files.
- `less {filename}`: Command line utility to display contents of a file or command output, one page at a time and allows navigating both forward and backward. It doesn’t read the entire file, leading to faster load times compared to text editors like vim or nano. Hence, it is mostly used for opening large files. `less +F` allows viewing growing files.
- `df`: Displays the disk space available and used on the mounted filesystems. Use `-h` for human-readable format for sizes.
- `du`: Displays disk usage of a file or directory, e.g. `du -hd2 {dirname}` where `-h` flag makes sizes human-readable and `-d{number}` sets the recursion depth for subdirectories. E.g., checking disk usage of users: `sudo du -hd1 /home | sort -hr`. This lists all users' disk usage in human-readable format (-h) upto depth 1 (-d1) and pipes it to the sort tool to arrange the list in reverse order by size (-r).
- inodes: Can check inodes of files with `ls -i` or `df -i`.
- `tree`: Show a directory and all subdirectories and files indented as a tree structure.
- Your home directory: `/home/{username}/`. Can navigate to home folder:
  ```bash
  cd
  cd ~
  cd /home/{username}
  ```
  **Note**: The root user's home directory, unlike normal users, is located at `/root/` not `/home/root/`.

#### Permissions

- `chmod`: To change permissions for a file. Can use symbols `u` (user that owns the file), `g` (the files group) and `o` (other users) and the permissions `r` (read), `w` (write) and `x` (execute).
  - Using `chmod u+x {filename}` will add execute permission for the owner of the file.
  - Can also encode the `ugo` permissions into 3 octals in range `{0-7}` each representing the `rwx` values, e.g., `chmod 764 {file}` gives read, write and execute permissions (111 = 7) to the user, read and write permissions (110 = 6) to the group, and read permissions (100 = 4) to others.
- `chown`: Changes the user and/or group that owns a file, e.g., `sudo chown {username}:{grpname} {filename}` will change the owner to `{username}` and the group to `{grpname}`.

#### Hard and Soft Links

A symbolic or soft link is a special sort of file that points to a different file's path (like shortcuts in Windows). A hard link, on the other hand, points to the exact inode (memory address) of the original file.

If you delete the original file, the soft link has no value, because it points to a non-existent file. But for hard links, even if you delete the original file, the hard link still points to the original file data.

In a soft link, the connection is a logical one, and not a duplication. So soft links can point at entire directories or cross file systems and link to files on remote computers. Hard links cannot do this.

However, hard links are not copies of a file since they don't duplicate the contents of the file. So if you modify the content of a copy, it has no effect on the original. However if you create a hard link to a file and change the content of either of the files, the change will be seen on both.

In a nutshell, a soft link:
- can cross the file system
- allows you to link between directories
- has separate inode number and file permissions than original file
- has only the path of the original file, not the contents
- created by `ln -s {source} {softlink}`

A hard link:
- can't cross the file system boundaries (only works on the same filesystem)
- can't link directories
- has the same inode number and permissions of the original file
- points to the actual contents of original file, so you can still view the contents, even if the original file is moved or removed
- created by `ln {source} {hardlink}`

#### File Systems

- The `mount` command attaches the filesystem on some device to your device's file tree. The `umount` command detaches it.
  - Attach with `mount {device} {dir}`.
  - Detach with `umount {dir}`. `umount {device}` will work, except if `device` was mounted on more than one directory.
- `fdisk`: Manipulate disk partition table
- `mkfs`: Build a Linux filesystem
- `lsblk`: List block devices

#### Compression

- `zip` (or `unzip`) can be used to compress (or extract) files into (or from) zip archives.
- `tar` can also create tar archives for compression. To compress: `tar -cvzf {filename.tar.gz} [{dirname/}]`. To extract: `tar -xvzf {filename.tar.gz}`.

#### Search

- `grep`: Search inside files for certain search patterns, e.g. `grep "search" *.txt` will look in all the files in the current directory ending with `.txt` for the string `search`. `grep` supports regular expressions which allows special letter combinations to be included in the search.
- `awk`: A programming language useful for searching and manipulating text files.
- `find`: Searches a directory and subdirectories for files matching certain patterns.
    - A cool usage of `find` to find all files owned by a user: `find / -user <username> &> <filename>`
- `whereis {command}` finds the location of a command. It looks through standard program locations until it finds the requested command.


## Networks

- `ping` utility is usually used to check if communication can be made with another host, e.g.:
  ```bash
  ping google.com
  ping 8.8.8.8
  ```
- `nmap` is a network exploration and scanning tool. It can return port and OS information about a host or a range of hosts. Running just `nmap` will display the options available as well as example usage.
- `hostname`: Displays the current hostname of the system. A superuser can set the hostname to a new one by supplying it as an argument (e.g. `hostname {newhost}`).
- `ifconfig`: Displays the network configuration details for the interfaces on the current system when run without any arguments. By supplying the name of an interface (e.g. `eth0` or `lo`) you can then alter the configuration.

#### SSH

- `ssh` denotes Secure Shell to connect to another remote machine using an encrypted network connection. Example usage: `ssh {username}@{ipaddress}`. Type in your password next and connect to the `{ipaddress}`.
- See [this](https://www.raspberrypi.org/documentation/remote-access/ssh/) page for a brief SSH tutorial.
- **Passwordless SSH**: To configure your machine to ssh into another remote machine without typing a password every time, you need to use an SSH key. To generate an SSH key:
    - **Checking for Existing SSH Keys**: First, run `ls ~/.ssh`. If you see files named `id_rsa.pub` or `id_dsa.pub` then you have keys set up already, so you can skip the 'Generate new SSH keys' step below.
    - **Generate new SSH Keys**: Run `ssh-keygen`. You will be asked where to save the key with a recommended default location (`~/.ssh/id_rsa`). Press `Enter`. You can enter an optional passphrase to encrypt the private SSH key, so that if someone else copied the key, they could not impersonate you to gain access. Type a passphrase here or leave it empty for no passphrase and press `Enter`. Run `ls ~/.ssh` and you should see the files `id_rsa` and `id_rsa.pub`. The `id_rsa` file is your private key to be kept on your machine. The `id_rsa.pub` file is your public key to share with remote machines that you connect to. When the machine you try to connect to matches up your public and private key, it will allow you to connect.
    - **Copy your Key to your remote machine**: Using the machine which you will be connecting from, append the public key to your `authorized_keys` file on the remote machine by sending it over SSH: `ssh-copy-id {username}@{ipaddress}` during which you need to authenticate with your password. Alternatively, you can copy the file manually over SSH: `cat ~/.ssh/id_rsa.pub | ssh {username}@{ipaddress} 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'`.
    - Now try `ssh {username}@{ipaddress}` to connect without a password prompt.
    - TODO: Find out more about basics of passwordless authentication, via `ssh-agent`, `ssh-add`, etc.

#### Download/upload files

- `scp`: Copies files between different machines using `ssh`. See [this](https://www.raspberrypi.org/documentation/remote-access/ssh/scp.md) link for a `scp` tutorial. Example usage: `scp {filename} {username}@{ipaddress}:{dirname}[/filename]`.
- Download a file from the web directly to the computer with `wget`, e.g. `wget https://www.raspberrypi.org/documentation/linux/usage/commands.md` will download the file to your computer as `commands.md`. See `wget --help` or `man wget` for more options.
- `curl`: Download or upload a file to/from a server. By default, it will output the file contents of the file to the screen.


## Processes

- `ps`: View processes that are currently running and their PIDs.
- `top`and `htop`: Real-time list of processes and their consumption of system resources.
- `glances`: Display all processes and resource (cpu, ram, hard disk etc.) consumption.
- Run a command/process in the background with `&`, freeing up the shell for future commands.

#### Killing processes

`kill` and `killall` can be used to terminate or send specific system signals to processes.

##### killall

- Terminate running processes based on name: `killall [pname]`. `killall` will terminate all programs that match the name specified.
- Without additional arguments, it sends `SIGTERM`, or signal number 15, which terminates running processes that match the name specified. You may specify a different signal as follows:
```bash
killall -s 9 [pname]
killall -KILL [pname]
killall -SIGKILL [pname]
killall -9 [pname]
```
All the above send the `SIGKILL` signal which is more successful at ending a particularly unruly processes.
- `-w` causes `killall` to wait until the process terminates.

##### kill

- Terminate processes based on PID: `kill {PID}`.
- Without options, `kill` sends `SIGTERM` to the PID specified and asks the application or service to shut itself down.
- Multiple PIDs and alternate system signals can be specified, e.g.:
```bash
kill -s KILL [PID]
kill -KILL [PID]
```
The above examples all send the `SIGKILL` signal to the PID specified.

##### pkill

- Kills process by process name, but can also match regular expressions. See `man pkill`.

**Signals**:

- List all available signals without description: `kill -l` or `killall -l`.
- If you need to convert a signal name into a signal number, or a signal number into a signal name: `kill -l 9` returns `KILL` or `kill -l kill` returns `9`.

## Users

- Add new user: `sudo adduser {username}`
    - Can also use `useradd` to add a new user. `useradd` is native binary compiled with the system. `adduser` is a more user friendly and interactive perl script which uses `useradd` in back-end, but no difference in features.
    - Can use the `-e, --expiredate` option to specify an expiry date, e.g.: `sudo useradd -e 2020-07-30 {username}`.
    - Contents of `/etc/skel/` are copied to any new user's home folder. Add or modify files such as `.bashrc` in `/etc/skel/` to your requirements for new users.
- See what groups a user is in: `groups {username}`
- Granting superuser priveleges to a user:
    - With usermod: `usermod -aG sudo {username}`
    - Edit the `/etc/sudoers` file or add them using `visudo`. `visudo` opens the `/etc/sudoers` configuration file in the system's default editor. Using visudo is recommended because it locks the file against multiple simultaneous edits and performs a sanity check on its contents before overwriting the file.
        - ```bash
          sudo visudo
          ```
        - Look for:
        ```
        root    ALL=(ALL:ALL) ALL
        ```
        - Below this line, copy the format by changing only the word "root" to reference the user that you would like to give sudo privileges to:
        ```
        root    ALL=(ALL:ALL) ALL
        {username} ALL=(ALL:ALL) ALL
        ```
    - To allow passwordless root access, change to `NOPASSWD: ALL`, e.g.:
        - ```
          root  ALL=(ALL:ALL) ALL
          {username} ALL = NOPASSWD: ALL
          ```
- Many important user settings can be changed via `usermod` like home directory, username, groups, shell, UID etc. See `man usermod`.
- Enter `su` to switch to a new user account: `su - {username}`
- Deleting a user account: `sudo deluser -r {username}`. `-r, --remove-home` removes their home folder also. After deleting a user, use `visudo` to remove `sudo` access for that user to prevent a new user created with the same name from being accidentally given sudo privileges.
- Change username of a user: `sudo usermod -l {new_username} {old_username}`
- Set password of a user: `sudo passwd {username}`. See more flags:
    - `-d, --delete`: Delete password of user
    - `-l, --lock`: Lock password of a user    
    - `-u, --unlock`: Unlock password of a user
    - `-S, --status`: Password status of a user
    - `-e, --expire`: Force expire passwordof a user
- User password expiry:
    - Set expiry date for user password: `chage -E 2020-07-30 {username}`. See [this](https://unix.stackexchange.com/questions/80968/how-can-i-create-automatically-expiring-user-accounts) link for more options.
    - View password expiry information for user: `chage -l {username}`.
- Unlock user and set new expiry: `usermod -U -e {YYYY-mm-dd} {username}`
- Change shell for a user: `sudo chsh {username}`.
- Change details of a user: `sudo chfn {username}`.
- Listing all local users: `cut -d: -f1 /etc/passwd`, where `cut` cuts each line of the file `/etc/passwd` by reading fields delimited by colon (`-d:`) and selecting the first field (`-f1`).


## Installing software (apt/apt-get)

Use `apt` or `apt-get` tool to install softwares. `apt` keeps a list of software sources on your system in a file: `/etc/apt/sources.list`.

- Update the list of available software: `sudo apt-get update`.
- Install a package: `sudo apt-get install {package}`.
- Remove a package: `sudo apt-get remove {package}`.
- Completely remove the package and its associated configuration files: `sudo apt-get purge {package}`.
- Can use a `-y` flag to answer the yes/no prompt with a yes in `install`, `remove`, `purge`.
- Upgrading packages:
    - Upgrade all packages: `sudo apt-get upgrade`.
    - Upgrade a certain package: `sudo apt-get install --upgrade {package}`.
- Search archives for a package with a given keyword: `apt-cache search {keyword}`.
- View more information about a package before installing it: `apt-cache show {package}`.

## Miscellaneous and Advanced Topics

- Scheduling tasks can be done with Cron. See a good Cron tutorial [here](https://www.raspberrypi.org/documentation/linux/usage/cron.md).
- In order to have a command or program run when the computer boots, you can add commands to the `rc.local` file. See more details [here](https://www.raspberrypi.org/documentation/linux/usage/rc-local.md).
- Use `systemd` to create services. See [this](https://www.raspberrypi.org/documentation/linux/usage/systemd.md) brief tutorial.
- Setting aliases with `.bashrc` and `bash_aliases`: see [tutorial](https://www.raspberrypi.org/documentation/linux/usage/bashrc.md).