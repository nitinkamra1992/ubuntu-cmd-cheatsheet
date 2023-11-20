# Cheatsheet of Ubuntu Commands

This repository contains a cheatsheet of commands for Ubuntu/Linux from various sources for quick reference. It also has links to useful tutorials for topics which cannot be summarized in a small cheatsheet.

#### Sources

- [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line)
- [MIT - The missing semester of your CS education](https://missing.csail.mit.edu/)
- [IBM's Linux tutorials](https://developer.ibm.com/tutorials/l-lpic1-map/)


## Basics of a Terminal/Shell

Terminal/Shell is used for typing and executing programs/commands. It is actually an interpreter which accepts a shell programming language syntax and looks to invoke programs if a written instruction cannot be matched to its expected language syntax.
- **echo**: Display a line of text, e.g. `echo hello world` or `echo "hello world"`.
- The arguments to a command/program are separated with spaces.
- Run `which <program-name>` to find out where you are running the program from.

#### Finding Help

- Use `man {command}` to display a manual for a command. Try `man man`! Press `q` to exit.
- Manual pages have a short description. Use `apropos {keyword}` to search the descriptions for instances of `keyword`.
- Some commands are not executables, but shell builtins.
  - `man` only helps for executables. For builtins, use `help {command}`.
  - `help -d` gives a list of builtins.
  - Use `type {command}` to find out if a command is an executable or a shell builtin or an alias. Try `type cd`, `type find`, `type ls`.
- Most programs also support `--help` argument to display help.
- `curl cheat.sh/{command}` gives a cheat sheet with common example usages of `command`.
- If you don't understand a command, can use [explainshell](https://explainshell.com/) to get a helpful breakdown for it.

#### Shell Scripting

- Commands can be combined together in a file which can then be executed.
- Bash is a popularly used language for scripting. Bash files have a `.sh` extension. You must make such a file executable by using `chmod` and then run it by typing `./{filename.sh}`.
- See the [shell scripting tutorial](https://www.shellscript.sh/) for more details.
- The [`nano` editor](https://www.nano-editor.org/) is a simple editor for basic terminal-editing (opening, editing, saving, searching). For power users in a text terminal, learning Vim (vi) is recommended. It is a hard-to-learn but venerable, fast, and full-featured editor.
- The `$SHELL` environment variable will tell you which shell you are using: `echo $SHELL`.
- `#` starts a comment in Bash, except if you use [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) (`#!`). `!` has a special meaning even within double-quoted (") strings, but Bash treats single-quoted strings (') differently. See the Bash [quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html) manual page for more information.

#### Variables

- Variables store information and pass it from the shell to programs.
- Programs look in the environment for particular variables and if they are found will use the values stored.
- Some variables are set by the system, some by the users, yet others by the shell.

- Variables are split into two categories: ENVIRONMENT variables and SHELL variables.
  - Shell variables apply only to the current instance of the shell and are used to set short-term working conditions
  - Environment variables have a farther reaching significance, and those set at login are valid for the duration of the session.
  - Convention: Environment variables have `UPPER CASE` and shell variables have `lower case` names.
  - Environment variables are set using the `setenv` command, displayed using the `printenv` or `env` commands, and unset using the `unsetenv` command. To show all environment variables, use: `printenv | less`.
  - Shell variables are both set and displayed using the `set` command. They can be unset by using the `unset` command.

- Examples:
  - The shell looks for locations to find programs in the order listed in your `$PATH` environment variable. The `$PATH` environment variable is a colon-separated list of directories. You can append more directories to it by: `export PATH="$PATH:{new_dir}"`.
  - The `$OSTYPE` environment variable has the current operating system that a user is using.
  - `$history` is a shell variable. The value of this means how many shell commands to save, allowing users to scroll back through all the commands they have previously entered. Try `echo $history`.

#### Bash Shortcuts

- Use **Tab** to complete arguments or list all available commands.
- Use `clear` to clear the terminal.
- Use `reset` to both clear the terminal and reset its settings to their default values.
- Use **ctrl-r** to search through command history (after pressing, type to search, press **ctrl-r** repeatedly to cycle through more matches, press **Enter** to execute the found command, or hit the right arrow to put the result in the current line to allow editing).
- Use **ctrl-a** to move cursor to beginning of line, **ctrl-e** to move cursor to end of line, **ctrl-k** to kill to the end of the line, **ctrl-l** to scroll down your terminal to a new command hiding all the content above. See `man readline` for all the default keybindings in Bash.
- For editing long commands, after setting your editor (for example `export EDITOR=vim`), **ctrl-x** **ctrl-e** will open the current command in an editor for multi-line editing.
- To see recent commands, use `history`. Follow with `!n` (where `n` is the command number) to execute again.
- Use the `{}` shell operator to bunch things together. Example: `mkdir -p /home/user/tmp/{dir1,anotherdir,similardir}`.
- To terminate an active running command, press **ctrl-c**.
- Use **ctrl-z** to suspend a process. When entered by a user, the currently running foreground process is sent a SIGTSTP signal, which generally causes the process to suspend its execution. The user can later continue the process execution (typically via `fg` command -- short for foreground) or run the process in background mode.
- Use `alias` to add simple aliases for complex commands. E.g., you can add `alias gp='git push'` to your `.bashrc` file to define an alias `gp` for the `git push` command. Search more on setting aliases with `.bashrc` and `bash_aliases`.

#### Shell Prompt

- Shell, when used from a terminal, issues a prompt before reading a command.
- Typically, a `%`` indicates the C shell (csh) and a `$`` indicates the Bourne shell (sh).
- Prompt can be controlled via special shell variables: PS1, PS2, PS3 and PS4. If set, the value is executed as a command prior to issuing each primary prompt.
  - PS1: The value of this parameter is expanded and used as the primary prompt string. The default value is \s-\v\$.
  - PS2: The value of this parameter is expanded as with PS1 and used as the secondary prompt string. The default is >.
  - PS3: The value of this parameter is used as the prompt for the select command.
  - PS4: The value of this parameter is expanded as with PS1 and the value is printed before each command bash displays during an execution trace. The first character of PS4 is replicated multiple times, as necessary, to indicate multiple levels of indirection. The default is +.
- `echo $PS1` to see the current prompt setting.
- Change your default prompt by altering your start-up files.
  - In the C shell, include in your `.login` file: `set prompt='newprompt'`.
  - In the Bourne shell, you need to set the value of the keyword shell variable PS1. This is just a variable that is declared and initialized by the shell at start-up. Include in your `.profile` file: `export PS1; PS1="newprompt"`.


## Constructing Commands

#### Redirecting Input and Output

- Most programs have an input stream (`stdin`), an output stream (`stdout`) and an error stream (`stderr`).
- By default the input stream defaults to the keyboard and the output stream defaults to the terminal.
- `>` and `<` can be used to redirect inputs and outputs of programs to a file.
  - `<` re-wires the input of the program to a file.
  - `>` re-wires the output of the program to a file.
  - `>>` appends the output to an existing file.
  - TODO: Learn about stdout and stderr.

- A pipe `|` redirects output of a command as the input for another command. E.g., to only show the first ten entries of the `ls` command, it can be piped through the head command: `ls | head`.
- Use `xargs` (or `parallel`) to build and execute command lines from standard input. This allows easily piping outputs of commands to others.
  - Note you can control how many processes execute in parallel with `-P`.
  - `-I{}` is handy for piping outputs into a subsequent command at the right location.

  Examples:
  ```bash
        find ./ -name '*.py' | xargs grep -nr <pattern>
        cat ./hostnames.txt | xargs -I{} ssh root@{} hostname
  ```
- Use `parallel` to build and execute shell command lines from standard input in parallel. Very useful to run jobs in parallel, without using loops in `sh` scripts. See the [manpage](https://manpages.ubuntu.com/manpages/impish/man1/parallel.1.html) for details.


## System Information

- `hostname`: Displays the current hostname of the system. A superuser can set the hostname to a new one by supplying it as an argument (e.g. `hostname {newhost}`).
- `lscpu`: Display details about your cpu architecture.
- `date`: Get the current date and time.


## Root User/Superuser (sudo)

- `root` user is the user with id 0 and full access to the system --> superuser.
- Run commands as the root user by using the `sudo` command (do as superuser).
- If you have to chain commands via pipe, `sudo` might only apply to the first command. Instead use `#` to run the full composed command as superuser, e.g.: `# echo 1 > /sys/net/ipv4_forward`.
- Alternatively, run a superuser shell: `sudo su`.
- Use `sudo -s` for a superuser shell.


## Files and Directories

- List information about the FILEs (default: current directory): `ls [OPTION] [FILE]`. Some useful flags for `ls`: 
    - `-a, --all`: List all files (including hidden files starting with a `.`)
    - `-d, --directory`: List directories themselves, not their contents
    - `-h, --human-readable`: With `-l` and/or `-s`, print human readable sizes
    - `-i, --inode`: Print the index number (inode) of each file
    - `-l`: Long list format to display additional information (permissions, owner, group, size, date and timestamp of last edit) for each file and directory.
      - The permissions are shown via `drwxrwxrwx` bits.
      - The first bit is `d` for a directory, `l` for a link and `-` otherwise.
      - The three groups of `rwx` bits display read, write and execute permissions for the owner, group and others respectively.
      - The permissions are interpreted as follows for files:
        - `r` means you can read or copy a file.
        - `w` means you can edit the file.
        - `x` means you can execute the file as a program.
      - These permissions are interpreted differently for directories.
        - `r` means you are allowed to see/list (`ls`) what files are in the directory.
        - `w` means you are allowed to rename, move, remove or create files in that directory. So even if you have write permissions on a file in that directory, you can modify it but cannot delete it unless you also have write permissions on its directory.
        - `x` means you are allowed to search/enter (`cd`) the directory to access the files in it. You need to have execute permissions on a directory and all its parents to be able to enter it.
      - A dash `-` means a lack of a permission.
    - `-R, --recursive`: List subdirectories recursively
    - `-s, --size`: Print the allocated size of each file, in blocks
- Change directory: `cd {path_to_dir}`. To go back to the previous working directory: `cd -`.
- Present working directory: `pwd`.
- Make directory: `mkdir {dirname}`. Use the `-p` option when creating a path to create parent directories as well.
- Delete file/directory:
    - `rm {filename}`
    - `rm -rf {dirname}`: `-r, -R, --recursive` flag deletes directories and their contents recursively; `-f, --force` never prompts and forcefully deletes even non-empty directories.
    - `rmdir {dirname}` removes only an empty directory.
- Copy: `cp {src_file} {dst_file}`. Can take `FILE FILE` or `FILE DIR` or `-r DIR DIR`.
- Move/Cut/Rename: `mv {src_file} {dst_file}`. Can take `FILE FILE` or `FILE DIR` or `-r DIR DIR`.
- `cat`: List the contents of file or multiple files, e.g. `cat {filename}` or `cat *.txt`. `cat` actually concatenates its input files into a single output.
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
  In `sh` scripts refer to the home directory as `$HOME`.
  **Note**: The root user's home directory, unlike normal users, is located at `/root/` not `/home/root/`.
- There are also special directories: `.` (current directory), `..` (parent directory) and `~` (home directory), which can be used with `cd`, `ls` or to specify relative paths in general.
- `tee {filename}` copies standard input to standard output, making a copy in `{filename}`. Useful for logging. E.g.: `echo hello | tee file.txt`.
- `xdg-open {filename}` opens files from a shell. By default, it'll use the default application for that file. If the file is in the form of a URL, the file will be opened as a URL. In MacOS, just use `open {filename}` instead.
- `touch {filename}` sets the modification and access times of files. If `{filename}` does not exist, it is created with default permissions.

#### Permissions

- `chmod`: For the owner to change permissions for a file.
  - Can use symbols `u` (user that owns the file), `g` (the files group) and `o` (other users) and the permissions `r` (read), `w` (write) and `x` (execute).
  - Using `chmod u+x {filename}` will add execute permission for the owner of the file.
  - Can also encode the `ugo` permissions into 3 octals in range `{0-7}` each representing the `rwx` values, e.g., `chmod 764 {file}` gives read, write and execute permissions (111 = 7) to the user, read and write permissions (110 = 6) to the group, and read permissions (100 = 4) to others.
- `chown`: For the superuser to change the user and/or group that owns a file.
  - Syntax: `chown [-R] [[user]][:group] target1 [[target2 ..]]` where `-R` allows recursive changes within a speified target directory.
  - E.g., `sudo chown {username}:{grpname} {filename}` will change the owner of `{filename}` to `{username}` and the group to `{grpname}`.

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
- `lsblk`: List block devices. Great for listing partitions and their sizes.

#### Compression

- `zip` (or `unzip`) can be used to compress (or extract) files into (or from) zip archives.
- `tar` can also create tar archives for compression. To compress: `tar -cvzf {filename.tar.gz} [{dirname/}]`. To extract: `tar -xvzf {filename.tar.gz}`.

#### Finding, Searching and Processing files

- `grep`: Search inside files for certain search patterns, e.g. `grep "search" *.txt` will look in all the files in the current directory ending with `.txt` for the string `search`. `grep` supports regular expressions which allows special letter combinations to be included in the search. Some useful flags:
  - `-n`: Print line numbers from files where pattern matches
  - `-r`: Recursive flag (for directories)
  - `-w`: Match only whole words
  - `-x`: Match only whole lines
  - `-v`: Select non-matching lines
  - `-f`: Specify the file containing the patterns to search for
  - `-i`: Ignore case in patterns and data
  - `-m`: Maximum number of matches to output
  - `-H`: Print file name with output lines
  - `-h`: Suppress the file name prefix with output
- `awk`: A programming language useful for searching and manipulating text files.
- `find {path}`: Searches a path and subdirectories for files matching certain patterns.
  - `-user`: Can specify owning user.
  - `-type`: Can specify `f` or `d` to find only files or directories respectively.
  - `-name {pattern}`: Can specify a pattern to match in the name.
  - `-iname {pattern}`: Case-insensitive specification to match in the name.
  - A cool usage of `find` to find all files owned by a user: `find / -user <username> &> <filename>`.
  - See this [link](http://www.devdaily.com/unix/edu/examples/find.shtml) for many useful examples of the `find` command.
- `whereis {command}` finds the location of a command. It looks through standard program locations until it finds the requested command.
- `cut` allows you to cut and retrieve parts of files. E.g.: `cut -d " " -f 2 file.txt` chunks `file.txt` by the space delimiter and returns the second field/element. Do `man cut` to read more about different ways to cut and retrieve.
- `sort {filename}` can sort all lines in a file. It can even take and sort lines from multiple files and merge them.
- `wc {filename}` gives the line, word and character/byte count in a file. Can also use flags `-w`, `-l`, `-c` and `-m` to extract word count, line count, byte count and character count independently.
- TODO: Learn to use `sed` for replacements in files.

#### Wildcards and Globbing

Glob or globbing is used to refer to an instance of pattern matching behavior. To specify the pattern of file names, we can use the glob command. Refer to the following link for more details: http://linux.about.com/library/cmd/blcmdln_glob.htm.

- `*` is a wildcard and matches 0 or more character(s) in a file (or directory) name. E.g., `ls *.py` lists all files ending in `.py` extension.
- `?` matches exactly one character.
- `[...]` matches any character in the character set. The set can be either a list of characters or a range separated with the hyphen character, e.g. `[1,2,3]` or `[a-z]`.

These characters, e.g., `*`, `?` etc. are called meta-characters and have special meanings depending on the program that sees them, including the shell itself. When we intend for the shell to pass these meta-chracters to a program as-it-is without the shell processing them in special ways, we use Quoting. You put quotes around the inputs containing the meta-characters to tell the shell that they are not special - as far as the shell is concerned. When you quote a character, you ask the shell to leave it alone - and pass it on unchanged to the program. See a [quick tutorial](http://www.tutorialspoint.com/unix/unix-quoting-mechanisms.htm) and the [full guide](http://www.grymoire.com/Unix/Quote.html). There are four methods of quoting:
- Single quote: All special characters between these quotes lose their special meaning.
- Double quote: Most special characters between these quotes lose their special meaning except: `$` for parameter substitution, \` for command substitution, `\$` to enable literal dollar sign, `\'` to enable literal single quotes, \\\` to enable literal back quotes, `\"` to enable embedded double quotes, `\\` to enable embedded backslashes.
- Backslash: Any character immediately following the backslash loses its special meaning.
- Back quote: Anything in between back quotes would be treated as a command and would be executed.


## Networks

- `ping` utility is usually used to check if communication can be made with another host, e.g.:
  ```bash
  ping google.com
  ping 8.8.8.8
  ```
- `nmap` is a network exploration and scanning tool. It can return port and OS information about a host or a range of hosts. Running just `nmap` will display the options available as well as example usage.
- `ifconfig`: Displays the network configuration details for the interfaces on the current system when run without any arguments (useful to check your ip address). By supplying the name of an interface (e.g. `eth0` or `lo`) you can then alter the configuration.
- `ip`: Show/manipulate routing, network devices, interfaces and tunnels. Typing `ip -c address` can show you the ip address for all the network interfaces on the current system (`-c` adds color to the output).
- `traceroute`: Print the route packets trace to a network host.
- `dig`: TODO: Learn more.
- `route`: TODO: Learn more.

#### SSH

- `ssh` denotes Secure Shell to connect to another remote machine using an encrypted network connection. Example usage: `ssh {username}@{ipaddress}`. Type in your password next and connect to the `{ipaddress}`.
- See [this](https://www.raspberrypi.com/documentation/computers/remote-access.html) page for a brief SSH tutorial.
- **Passwordless SSH**: To configure your machine to ssh into another remote machine without typing a password every time, you need to use an SSH key. To generate an SSH key:
    - **Checking for Existing SSH Keys**: First, run `ls ~/.ssh`. If you see files named `id_rsa.pub` or `id_dsa.pub` then you have keys set up already, so you can skip the 'Generate new SSH keys' step below.
    - **Generate new SSH Keys**: Run `ssh-keygen`. You will be asked where to save the key with a recommended default location (`~/.ssh/id_rsa`). Press `Enter`. You can enter an optional passphrase to encrypt the private SSH key, so that if someone else copied the key, they could not impersonate you to gain access. Type a passphrase here or leave it empty for no passphrase and press `Enter`. Run `ls ~/.ssh` and you should see the files `id_rsa` and `id_rsa.pub`. The `id_rsa` file is your private key to be kept on your machine. The `id_rsa.pub` file is your public key to share with remote machines that you connect to. When the machine you try to connect to matches up your public and private key, it will allow you to connect.
    - **Copy your Key to your remote machine**: Using the machine which you will be connecting from, append the public key to your `authorized_keys` file on the remote machine by sending it over SSH: `ssh-copy-id {username}@{ipaddress}` during which you need to authenticate with your password. Alternatively, you can copy the file manually over SSH: `cat ~/.ssh/id_rsa.pub | ssh {username}@{ipaddress} 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'`.
    - Now try `ssh {username}@{ipaddress}` to connect without a password prompt.
    - TODO: Find out more about basics of passwordless authentication, via `ssh-agent`, `ssh-add`, etc.

#### Download/upload files

- `scp`: Copies files between different machines using `ssh`. Example usage: `scp {filename} {username}@{ipaddress}:{dirname}[/filename]`.
- Download a file from the web directly to the computer with `wget`, e.g. `wget https://www.raspberrypi.org/documentation/linux/usage/commands.md` will download the file to your computer as `commands.md`. See `wget --help` or `man wget` for more options. You can also download full websites this way with `wget -r {url}`.
- `curl`: Download or upload a file to/from a server. By default, it will output the file contents to the screen.


## Processes

- `ps`: View processes that are currently running and their PIDs.
- `pstree -p` is a helpful display of the process tree.
- `top`and `htop`: Real-time list of processes and their consumption of system resources.
- `glances`: Display all processes and resource (cpu, ram, hard disk etc.) consumption.
- Run a command/process in the background independently of the shell by including an `&` at the end of the command, freeing up the shell for future commands.
  - To check the status of your job, use the command `ps`.
  - To bring a background process to the foreground, use the command `fg`.
  - If there are more than one job suspended in the background, use the command: `fg {job_number}`.
  - The job numbers are shown in the first column of the output of the `jobs` command.

#### Killing processes

`kill`, `pkill` and `killall` can be used to terminate or send specific system signals to processes.

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

- Terminate processes based on PID: `kill {PID}` or job_number: `kill {job_number}`.
- Without options, `kill` sends `SIGTERM` to the PID specified and asks the application or service to shut itself down.
- Multiple PIDs and alternate system signals can be specified, e.g.:
```bash
kill -s KILL [PID]
kill -KILL [PID]
```
The above examples all send the `SIGKILL` signal to the PID specified.

##### pkill

- Kills process by process name, but can also match regular expressions. See `man pkill`.

#### Signals

- List all available signals without description: `kill -l` or `killall -l`.
- If you need to convert a signal name into a signal number, or a signal number into a signal name: `kill -l 9` returns `KILL` or `kill -l kill` returns `9`.

#### Executing programs without forking a new process

The `exec` command replaces the current shell process with the specified command. Normally, when you run a command a new process is spawned (forked). The `exec` command is executed in place of the current shell without creating a new process. The command implements Unix exec system call.
- `exec [command] [arg ...]`

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
    - `-e, --expire`: Force expire password of a user
- User password expiry:
    - Set expiry date for user password: `chage -E 2020-07-30 {username}`. See [this](https://unix.stackexchange.com/questions/80968/how-can-i-create-automatically-expiring-user-accounts) link for more options.
    - View password expiry information for user: `chage -l {username}`.
- Unlock user and set new expiry: `usermod -U -e {YYYY-mm-dd} {username}`
- Change shell for a user: `sudo chsh {username}`.
- Change details of a user: `sudo chfn {username}`.
- Listing all local users: `cut -d: -f1 /etc/passwd`, where `cut` cuts each line of the file `/etc/passwd` by reading fields delimited by colon (`-d:`) and selecting the first field (`-f1`).


## Installing software

Use `apt` or `apt-get` tool to install softwares. `apt` keeps a list of software sources on your system in a file: `/etc/apt/sources.list`.
- Update the list of available software: `sudo apt update`.
- Install a package: `sudo apt install {package}`.
- Remove a package: `sudo apt remove {package}`.
- Completely remove the package and its associated configuration files: `sudo apt purge {package}`.
- Can use a `-y` flag to answer the yes/no prompt with a yes in `install`, `remove`, `purge`.
- Upgrading packages:
    - Upgrade all packages: `sudo apt upgrade`.
    - Upgrade a certain package: `sudo apt install --upgrade {package}`.
- Search archives for a package with a given keyword: `apt-cache search {keyword}`.
- View more information about a package before installing it: `apt-cache show {package}`.

Use `pip` to install `python` packages. See a detailed tutorial [here](https://realpython.com/what-is-pip/). Some common usage syntaxes below.
- `pip install {package}`: Install a python `package`.
- `pip list`: List all installed packages in the environment and their version numbers.
- `pip show {package}`: Display info about a `package`.
- `pip install -i {index} {package}`: Install `package` from a different index as opposed to the default PyPI index.
- `pip freeze > requirements.txt`: Create a `requirements.txt` file with all packages installed in the current environment and their version numbers.
- `pip install -r requirements.txt`: Install all packages from `requirements.txt`.


## Miscellaneous Stuff

- Convert an image sequence into a video: `ffmpeg -framerate 30 -pattern_type glob -i '*.jpg' -c:v libx264 -pix_fmt yuv420p out.mp4`.
- Merge multiple jpgs vertically: `convert pic1.jpg pic2.jpg pic3.jpg -append pic.jpg`
- Show a zoomable world map: `telnet mapscii.me`.
- `nvidia-smi`: Monitor nvidia gpu usage.
- `watch`: Repeat a command periodically and update output on terminal. Example: `watch -n 5 nvidia-smi` monitors the gpu usage every 5 seconds.

#### Coding

- Version control tools are useful for coding. Learn more about `git` by googling for a good tutorial.
- `cloc` can be used to count lines of code in a directory, project or file. Examples:
  - `cloc ./` for a directory
  - `cloc $(git ls-files)` for a git repo


## Some Commands for MacOS

Here are some interesting commands for MacOS from [Advanced macOS Command-Line Tools](https://saurabhs.org/advanced-macos-commands):

#### caffeinate - Set Mac sleep behavior

Running caffeinate with no arguments prevents your Mac from going to sleep as long as the command continues to run.
- `caffeinate -u -t <seconds>` prevents sleep for the specified number of seconds.
- Adding the `-d` flag also prevents the display from going to sleep.
- Specifying an existing process with `-w <pid>` automatically exits the caffeinate command once the specified process exits.
- Passing a command with `caffeinate <command>` starts the given command in a new process and prevents sleep until that process exits.

#### pbcopy, pbpaste - Interact with system clipboard

- `<command> | pbcopy` copies the output of the command to the clipboard.
- `pbpaste` outputs the contents of the clipboard to `stdout`.

#### networkQuality - Measure Internet speed

- Run `networkQuality` to run an Internet speed test from your terminal.
- Add the `-v` flag to view more detailed information.
- Use the `-I` flag to run the network test on a specific network interface.

#### sips - Image manipulation

- `sips -z <height> <width> <image>` resizes the specified image, ignoring the previous aspect ratio.
- `sips -Z <size> <image>` resizes the largest side of the specified image, preserving the aspect ratio.
- `sips -c <height> <width> <image>` crops the specified image to the given dimensions (relative to the center of the original image).
- `sips -r <degrees> <image>` rotates the image by the specified degrees.
- By default, sips will overwrite the input image. Use the `-o` flag to specify a different output file path (must have the same file extension as the input image).

#### open - Open files and applications

- `open -a <app> <file>` opens the given file with the specified application.
- Add the `-g` flag to open the file in the background, without losing focus from the current application.
- `open .` opens the current directory in a new Finder window.
- `open -R <file>` reveals the given file in a new Finder window.

#### textutil - Document file converter

- `textutil` can convert files to and from Microsoft Word, plain text, rich text, and HTML formats.
- `textutil -convert html journal.doc` converts journal.doc into journal.html.
- The possible values for `-convert` are: `txt`, `html`, `rtf`, `rtfd`, `doc`, `docx`.

#### mdfind, mdls - Search with Spotlight

- `mdfind <query>` performs a keyword-based Spotlight search with the given query.
- `mdfind kMDItemAppStoreHasReceipt=1` finds all apps installed from the Mac App Store.
- `mdfind -name <name>` searches for all files matching the given name.
- The `-onlyin <dir>` flag restricts the search to the given directory.
- `mdls <file-path>` prints all Spotlight metadata associated with the given file.

#### screencapture - Take screenshots

- `screencapture -c` takes a screenshot and copies it to the clipboard.
- `screencapture <file>` takes a screenshot and saves it to the given file.
- Add the `-T <seconds>` flag to take the screenshot after the given delay in seconds.

#### taskpolicy - Control scheduling of processes

- `taskpolicy -b <command>` starts executing the given command in the background. On Apple silicon Macs, the process will only run on the efficiency cores.
- `taskpolicy -b -p <pid>` downgrades an existing process to run in the background.
- `taskpolicy -B -p <pid>` removes the specified process from running in the background. On Apple silicon Macs, the process may now run on the efficiency or performance cores. Note that this only works on processes that have been downgraded to the background, and not processes that started in the background.
- `taskpolicy -s <command>` starts the given command in the suspended state. This is useful to allow a debugger to attach to the process right at the start of execution.

#### say - Text-to-speech engine

- `say <message>` announces the given message.
- `say -f input.txt -o output.aiff` creates an audiobook from the given text file.
- `say -v '?' <message>` will print out all possible voices. You can replace the `?` with a voice.

#### pmset - Configure power management

- `pmset -g` prints all available power configuration information.
- `pmset -g assertions` displays information about power-related assertions made by other processes. This can be useful for finding a process that is preventing your Mac from going to sleep.
- `pmset -g thermlog` displays information about any processes that have been throttled (useful when running benchmarks).
- `pmset displaysleepnow` immediately puts the display to sleep without putting the rest of the system to sleep.
- `pmset sleepnow` immediately puts the entire system to sleep.

#### networksetup - Configure network settings

- `networksetup -listnetworkserviceorder` prints a list of available network services.
- `networksetup -getinfo <networkservice>` prints information about the specified network service.
- `networksetup -getdnsservers <networkservice>` prints the DNS servers for the specified network service.
- `networksetup -setairportnetwork <device> <network> [password]` joins the specified Wi-Fi network. (In most cases, the argument should be "en0".)

#### softwareupdate - Manage OS updates

- `softwareupdate --list` prints out available software updates.
- `sudo softwareupdate -ia` installs all available updates.
- `softwareupdate --fetch-full-installer --full-installer-version <version>` tries to download the full installer of the specified macOS version to /Applications.

#### system_profiler - View system information

- `system_profiler` by default prints all available system information, which is usually overwhelming.
- `system_profiler <datatype>` only prints information about the given sub-system.
- `system_profiler -listDataTypes` lists all available sub-systems to get information from.

Some particularly useful ones:
- `system_profiler SPHardwareDataType` prints an overview of the hardware of the current machine, including its model name and serial number.
- `system_profiler SPSoftwareDataType` prints an overview of the software of the current machine, including the exact macOS version number.
- `system_profiler SPPowerDataType` prints power and battery information, including the current AC wattage and battery cycle count.
- `system_profiler SPDeveloperToolsDataType` prints the currently active version of the Xcode developer tools and SDK.


## Advanced Topics

- Scheduling tasks can be done with Cron. See a Cron tutorial [here](https://www.freecodecamp.org/news/cron-jobs-in-linux/).
- In order to have a command or program run when the computer boots, you can access run control (rc) or add commands to the `rc.local` file. See more details [here](https://docs.oracle.com/cd/E19683-01/806-4073/6jd67r96g/index.html).
- Use `systemd` to create services. See [this](https://medium.com/@benmorel/creating-a-linux-service-with-systemd-611b5c8b91d6) brief tutorial.