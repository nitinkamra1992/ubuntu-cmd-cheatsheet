# Cheatsheet of useful Ubuntu Commands

[Raspberry Pi Documentation on Linux](https://www.raspberrypi.org/documentation/linux/) is a great resource to learn about Ubuntu/Linux from scratch. This repository contains a cheatsheet of commands from it (and other resources) for quick reference.

## Root User/Superuser (sudo)

- Run commands as the root user by using the `sudo` command before the program you want to run.
- Run a superuser shell: `sudo su`.
- Use `sudo -s` for a superuser shell.

## Home

- Home directory: `/home/{username}/`.
- Navigating to home folder:
  ```bash
  cd
  cd ~
  cd /home/{username}
  ```
**Note**: When logged in as the root user, typing `cd` or `cd ~` will take you to the root user's home directory; unlike normal users, this is located at `/root/` not `/home/root/`.

## Basic Commands

- 'echo': Display a line of text, e.g. `echo hello world`.

#### Files and directories

- List information about the FILEs (default: current directory): `ls [OPTION] [FILE]`.
- Some useful flags for `ls`: 
    - `-a, --all`: List all (including hidden files starting with a `.`) files
    - `-d, --directory`: List directories themselves, not their contents
    - `-h, --human-readable`: With `-l` and/or `-s`, print human readable sizes
    - `-l`: Long list format to display additional information (permissions, owner, group, size, date and timestamp of last edit) for each file and directory
    - `-R, --recursive`: List subdirectories recursively
    - `-s, --size`: Print the allocated size of each file, in blocks
- Change directory: `cd {path_to_dir}`.
- Present working directory: `pwd`.
- Make directory: `mkdir {dirname}`.
- Delete file/directory:
    - `rm {filename}`
    - `rm -rf {dirname}`: `-r, -R, --recursive` flag deletes directories and their contents recursively; `-f, --force` never prompts and forcefully deletes even non-empty directories.
- Copy: `cp {src_file} {dst_file}`. Can take `FILE FILE` or `FILE DIR` or `-r DIR DIR`.
- Move/Cut: `mv {src_file} {dst_file}`. Can take `FILE FILE` or `FILE DIR` or `-r DIR DIR`.
- Use `man {command}` to show the manual page for a command. Try `man man`!
- `cat`: List the contents of file or multiple files, e.g. `cat {filename}` or `cat *.txt`.
- `head {filename}`: Displays the beginning of a file. Use `-n` to specify the number of lines to show (default: 10) or `-c` to specify the number of bytes.
- `tail {filename}`: Displays the end of a file. The starting point in the file can be specified either through `-c` for bytes or `-n` for number of lines.
- `df`: Displays the disk space available and used on the mounted filesystems. Use `-h` for human-readable format for sizes.
- `du`: Displays disk usage of a file or directory, e.g. `du -hd2 {dirname}` where `-h` flag makes sizes human-readable and `-d{number}` sets the recursion depth for subdirectories.
- `tree`: Show a directory and all subdirectories and files indented as a tree structure.

#### Permissions

- `chmod`: To change permissions for a file. Can use symbols `u` (user that owns the file), `g` (the files group) and `o` (other users) and the permissions `r` (read), `w` (write) and `x` (execute). Using `chmod u+x {filename}` will add execute permission for the owner of the file.
- `chown`: Changes the user and/or group that owns a file, e.g., `sudo chown {username}:{grpname} {filename}` will change the owner to `{username}` and the group to `{grpname}`.

#### Compression

- `zip` (or `unzip`) can be used to compress (or extract) files into (or from) zip archives.
- `tar` can also create tar archives for compression. To compress: `tar -cvzf {filename.tar.gz} [{dirname/}]`. To extract: `tar -xvzf {filename.tar.gz}`.

#### Search

- `grep`: Search inside files for certain search patterns, e.g. `grep "search" *.txt` will look in all the files in the current directory ending with `.txt` for the string `search`. `grep` supports regular expressions which allows special letter combinations to be included in the search.
- `awk`: A programming language useful for searching and manipulating text files.
- `find`: Searches a directory and subdirectories for files matching certain patterns.
- `whereis {command}` finds the location of a command. It looks through standard program locations until it finds the requested command.

#### Pipes

A pipe allows the output from one command to be used as the input for another command. The pipe symbol is a vertical line `|`. For example, to only show the first ten entries of the `ls` command it can be piped through the head command  `ls | head`.

#### Shell Scripting

Commands can be combined together in a file which can then be executed (preferably with `.sh` extension if you are using `bash` terminal). You must make such a file executable by using `chmod` and then run it by typing `./{filename.sh}`. To terminate an active command running in the terminal, press `Ctrl + C`. See the [shell scripting tutorial](https://www.shellscript.sh/) for more details.

#### Download/Upload

- `ssh` denotes Secure Shell to connect to another computer using an encrypted network connection. See [this](https://www.raspberrypi.org/documentation/remote-access/ssh/) page for a brief SSH tutorial.
- `scp`: Copies files between different machines using `ssh`. See [this](https://www.raspberrypi.org/documentation/remote-access/ssh/scp.md) link for a `scp` tutorial. Example usage: `scp {filename} {username}@{ipaddress}:{dirname}[/filename]`.
- Download a file from the web directly to the computer with `wget`, e.g. `wget https://www.raspberrypi.org/documentation/linux/usage/commands.md` will download the file to your computer as `commands.md`. See `wget --help` or `man wget` for more options.
- `curl`: Download or upload a file to/from a server. By default, it will output the file contents of the file to the screen.

#### Networking
- `ping` utility is usually used to check if communication can be made with another host, e.g.:
  ```bash
  ping google.com
  ping 8.8.8.8
  ```
- `nmap` is a network exploration and scanning tool. It can return port and OS information about a host or a range of hosts. Running just `nmap` will display the options available as well as example usage.
- `hostname`: Displays the current hostname of the system. A superuser can set the hostname to a new one by supplying it as an argument (e.g. `hostname {newhost}`).
- `ifconfig`: Displays the network configuration details for the interfaces on the current system when run without any arguments. By supplying the name of an interface (e.g. `eth0` or `lo`) you can then alter the configuration.

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
- [Text editors](https://www.raspberrypi.org/documentation/linux/usage/text-editors.md) in Linux.
- In order to have a command or program run when the computer boots, you can add commands to the `rc.local` file. See more details [here](https://www.raspberrypi.org/documentation/linux/usage/rc-local.md).
- Use `systemd` to create services. See [this](https://www.raspberrypi.org/documentation/linux/usage/systemd.md) brief tutorial.
- Setting aliases with `.bashrc` and `bash_aliases`: see [tutorial](https://www.raspberrypi.org/documentation/linux/usage/bashrc.md).