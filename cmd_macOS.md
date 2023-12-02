# Some Commands for MacOS

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