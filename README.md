yakuake-session - A script to create new yakuake sessions from command-line.<br />
Copyright 2010-2018 Jesús Torres \<jmtorres@ull.es\>

## What is yakuake-session?

yakuake-session is a Bash script that allows to create new sessions in Yakuake
terminal emulator from command line. I made it mainly to get a better
integration of Yakuake in KDE desktop environment. For example, thanks to it
Yakuake can replace Konsole in "Open terminal here" action in Dolphin or we
can setup a menu similar to Konsole Profiles widget but using Yakuake instead
of Konsole.

## Bugs & known issues

 * Bug [#1177823](http://bugs.launchpad.net/ubuntu/+source/qt4-x11/+bug/1177823)
breaks the command `qdbus` in Kubuntu 13.04/13.10. It can be fixed installing
`qt4-default` package.

 * Requires wmctrl to change focus if yakuake is already open. Get it from the
repo e.g. `apt install wmctrl`

 * Fish is not a POSIX compliant shell, so yakuake_session detects if it is the
user default shell and applies some fixes. If the autodetection doesn't work
properly, open the script and set the variable FISH_SHELL in the first lines to
1 to apply the fixes unconditionally.

## Installation

Clone this repository.

```
git clone https://github.com/aplatanado/yakuake-session.git
```

Copy the yakuake-session script to `~/bin`, `/usr/bin` or some other directory
in `$PATH` variable, if we want to avoid indicating the path of the script when
invoked. For example, on Ubuntu:

```
sudo cp yakuake-session /usr/bin
```

## Usage

To invoke yakuake-session:

```
$ yakuake-session
```

Without arguments, yakuake-session creates a new session in the currently
running Yakuake terminal emulator.

The option `-e` allows to indicate a command to execute in the new session.

```
$ yakuake-session -e ls
```

The argument `-t` sets the title for the new tab.

```
$ yakuake-session -t "Title"
```

The session working directory is changed to the directory where yakuake-session
was called, before execute the command. If we want to launch the command from
user's home directory then we would use the option `-h`.

```
$ yakuake-session -h -e ls
```

If the command requires some arguments, they are taken from non-option
arguments passed to yakuake-session. That means that if some arguments for the
command begin with `-`, they must passed to yakuake-session after `--`.

```
$ yakuake-session -h -e ssh -- -X user@example.com
```

Another useful option is `--workdir`. It allows to change the working directory
before execute the command.

```
$ yakuake-session --workdir /tmp -e ls
```

yakuake-session has many other options that were shown in help.

```
$ yakuake-session --help

Usage: yakuake-session [options] [args]

Options:
  --help                    Show help about options.
  -h, --homedir             Set the working directory of the new tab to the user's home.
  -w, --workdir <dir>       Set the working directory of the new tab to 'dir'
  --hold, --noclose         Do not close the session automatically when the command ends.
  -p <property=value>       Change the value of a profile property (only for KDE 4).
  -q                        Do not open yakuake window.
  -t <title>                Set the title of the new tab
  -e <cmd>                  Command to execute. This option will catch all following arguments, so use it as the last option.
  --fish | --nofish         Override default shell autodetection to enable or disable the fish shell support.
  --debug                   Show yakuake_session debug information.

Arguments:
  args                      Arguments passed to command (for use with -e).
```

## Action in Dolphin

Dolphin provides the action "Open terminal here" that opens a Konsole terminal
emulator in the specified folder. This behavior can be changed to use Yakuake
instead of Konsole coping `konsolehere.desktop` into KDE Service Menus.

```
# KDE 4
cp ServiceMenus/konsolehere.desktop ~/.kde/share/kde4/services/ServiceMenus/
# KDE 5
cp ServiceMenus/konsolehere.desktop ~/.local/share/kservices5/ServiceMenus/
cp ServiceMenus/konsolehere.desktop /usr/share/kservices5/ServiceMenus/ # if using Manjaro Arch Linux
```

If we do not want to change the behavior of "Open terminal here", then copy
`yakuakehere.desktop` instead to add the new action "Open yakuake here" to
Dolphin.

```
# KDE 4
cp ServiceMenus/yakuakehere.desktop ~/.kde/share/kde4/services/ServiceMenus/
# KDE 5
cp ServiceMenus/yakuakehere.desktop ~/.local/share/kservices5/ServiceMenus/
cp ServiceMenus/yakuakehere.desktop /usr/share/kservices5/ServiceMenus/ # if using Manjaro Arch Linux
```

## Quick Access Menu

Konsole Profiles is a Plasma widget that allows to open a new terminal window,
configured according to a select profile, and automatically execute a command
in it. We can get something similar but for Yakuake using the QuickAccess
widget. We only have to make a directory and setup a QuickAccess widget
instance to use it as origin (I also like to disable the browsing). Then we
add some "Link to Application" to that directory, such that each one use
yakuake-session to create a new Yakuake session and to execute the command
that we want.

The file `examples/username@example.com.desktop` contains an example that launch
a ssh client in a new Yakuake session.


-- Jesús Torres \<jmtorres@ull.es\>
