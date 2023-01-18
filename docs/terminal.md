# Text Processing

## awk

```sh
## GENERAL STUFF
# print first column
awk '{print $1}'

# print last column
awk '{print $NF}'

# print 2nd last column
awk '{print $NF-2}'

# use custom delimiter
awk 'BEGIN{FS=":"} {print $1}'


## CONDITIONAL STUFF
# using IF conditions
awk '{if ($1=="chr21" && $4>80) {print $0}}'

# print if a substring is part of the string
echo "snowball" | awk '$1 ~ /snow/'

# print if a substring is NOT part of the string
echo "snowball" | awk '$1 !~ /rain/'


## COOL TRICKS
# print row number
awk '{print NR ") " $1 " -> " $(NF-2)}'

# slice range of columns
Better to use cut command. See elsewhere in this page.
```

[Source](https://gregable.com/2010/09/why-you-should-know-just-little-awk.html)


## cut

```sh
# print from 3rd column
cut -f 3- INPUTFILE

# print upto 3rd column
cut -f -3 INPUTFILE
```

[Source](https://stackoverflow.com/a/1602220/3998252)


## paste

Allows combining file as columns side by side.

```sh
# example
$ cat a
A
B
C
D
$ cat b
1
2
3
4
$ paste a b
A   1
B   2
C   3
D   4
```

[Source](https://unix.stackexchange.com/a/117590)



# Numeric, textual and statistical operations

## datamash

Commandline tool to get statistics.
[Documentation](https://www.gnu.org/software/datamash/manual/datamash.html)


# Prettifying

## Pretty json

```sh
curl 'url' | jq . > out.json
```

Warning: Apparently many pretty json formatters may have [problem handling very large and very small numbers](http://stackoverflow.com/questions/352098/how-can-i-pretty-print-json#comment52647558_15231463).


## Viewing CSV files in terminal

```sh
csvtool readable filename
```


## Preserving colors when redirecting to file

Simply insert `unbuffer` before any command to make it think it is writing to an interactive output even if it is actually piping into another executable.

If colors are not visible with `less`, use `less -R`.

Works with `nohup` as well.

[Source](https://superuser.com/a/751809)


# Version control

## Git submodule

### Create submodule

```sh
# basic command
git submodule add <git_repo_url_>

# to store it in particular path
git submodule add <git_repo_url_> <local_path>

# to use particular branch instead of master
git submodule add -b <branch> <git_repo_url_>
```

### To checkout particular release/commit

```sh
cd <submodule_directory>
git checkout v1.0
```
[Source](https://stackoverflow.com/a/1778247/3998252)

### Remove submodule

No one-stop command. Follow the [solution here](https://stackoverflow.com/a/16162000/3998252).


### Fetch repo with submodules

```sh
git clone --recursive <git_url>
```

# Others

## Recursive download of folder contents using wget

```sh
wget -r url
```

Above command preserved the directory structure, but instead of downloading from current directory specified in URL, it downloads at least few directories up in the directory structure. I had to identify the folder and copy it. Minor hiccup but works well.

[Source](http://stackoverflow.com/questions/113886/how-to-recursively-download-a-folder-via-ftp-on-linux)



## sshfs in Mac

Install appropriate OSX Fuse and then use following commands:

```sh
# to mount from root
sudo sshfs -o allow_other,defer_permissions username@address.org:/ /mnt/cluster/

# to mount home dir
sudo sshfs -o allow_other,defer_permissions username@address.org:/remote/path/to/mount  /mnt/cluster/

Note: Since 'sudo'ing, first use local system's password and then it will ask for remote server's password.

# to unmount
sudo umount /mnt/cluster

# if 'resource busy' error when trying unmount, use -f
sudo umount -f /mnt/cluster
```


 ## md5sum

Creating checksum

```sh
# create checksum
md5sum <filename>

# can use multiple files at once
md5sum <filename1> <filename2> <filename3>
```

Checking for integrity

```sh
# using file contaiing checksum strings
md5sum -c <md5_file>

# checksum with string directly
md5sum -c <<<"[checksum_string] [filename]"
```


## Redirecting output to both stdout and file

If you only care about stdout:

    ls -a | tee output.file

If you want to include stderr, do:

    program [arguments...] 2>&1 | tee outfile

`2>&1` redirects channel 2 (stderr/standard error) into channel 1 (stdout/standard output), such that both is written as stdout. It is also directed to the given output file as of the `tee` command.

Furthermore, if you want to _append_ to the log file, use `tee -a` as:

    program [arguments...] 2>&1 | tee -a outfile

[Source](https://stackoverflow.com/a/418899/3998252)


## Save HTML webpage as screenshot

[Multiple ways to save webpage as a screenshot image](http://cutycapt.sourceforge.net/)

[pageres](https://github.com/sindresorhus/pageres-cli) worked well.

```sh
pageres google.com

pageres google.com 2048x1080
```

## rclone

### copy data on the server side

```sh
SRC="path/to/source"
DEST="path/to/dest"

# copies data. Here box is name used for the client when configuring rclone.
rclone copy  --log-file=copy.log --progress --verbose box:$SRC box:$DEST

# checks for file integrity
rclone check --log-file=check.log --progress --verbose box:$SRC box:$DEST
```

## rsync

### Copy with resuming enabled after interruption

```sh
$ CLU="xxxx@host.address.edu"
$ SOURCE_DIR="xxxx"
$ DEST_DIR="xxxx"
$ PARTIAL_DIR="${DEST_DIR}/.rsync-partial"
$ LOG_F="log.txt"

$ rsync --archive --human-readable --progress \
    -v -v \
    --partial-dir=$PARTIAL_DIR \
    --rsh=ssh \
    $SOURCE_DIR \
    $CLU:$DEST_DIR \
    >> $LOG_F
```

- [See here](https://serverfault.com/a/141778) for what `--archive` flag does.
- `--partial-dir` flag is to help with [resuming after interruption](https://unix.stackexchange.com/a/252969/339199).


## module

### Common commands

| command                    | description                                                                                                          |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| module list                | List currently loaded modules.                                                                                       |
| module avail               | List available packages.                                                                                             |
| module spider              | List available packages in a different format.                                                                       |
| module help [modulefile]   | Description of specified module.                                                                                     |
| module show [modulefile]   | Displays information about specified module, including environment changes, dependencies, software version and path. |
| module load [modulefile]   | Loads module or specifies which dependencies have not been loaded.                                                   |
| module unload [modulefile] | Unloads specified module from environment.                                                                           |
| module purge               | Unloads all loaded modules                                                                                           |

[Source](http://www.bu.edu/tech/support/research/software-and-programming/software-and-applications/modules/)


## xargs

```sh
# useful options
-t      Print the command line on the standard error output before executing it.
-p      Prompt the user about whether to run each command line and read a line from the terminal. Only run the command line if the response starts with ‘y’ or ‘Y’. Implies ‘-t’.
-n      Use at most max-args arguments per command line.
-d      delimiter
-P      Run simultaneously up to max-procs processes at once; the default is 1. If max-procs is 0, xargs will run as many processes as possible simultaneously.
-I replace-str    Replace occurrences of replace-str in the initial arguments with names read from standard input. Also, unquoted blanks do not terminate arguments; instead, the input is split at newlines only.
```

```sh
# Examples

# "find" outputs .err files modified in last 24 hours.
# For each of this file (using -n 1), xargs runs a chained command
# Notice percent sign is used to specify arguments multiple times

find *.err -mtime -1 | xargs -I {} -n 1 sh -c "echo {}; ./del.py -s {} | grep TIMEOUT"
```

## fd

[A simple, fast and user-friendly alternative](https://github.com/sharkdp/fd) to `find`. While it does not aim to support all of find's powerful
functionality, it provides sensible (opinionated) defaults for a majority of use cases.

Few things to note:
- case-insensitive by default
- Smart case: the search is case-insensitive by default. It switches to case-sensitive if the pattern contains an uppercase character.
- Ignores hidden directories and files, by default.
- Ignores patterns from your .gitignore, by default.
- Allows specifying time/duration in human readable format. Example: `--newer 2weeks`.

Usage:
```sh
fd [FLAGS/OPTIONS] [<pattern>] [<path>...]
```

Some useful flags/options:
```sh
FLAGS:
    -H, --hidden            Search hidden files and directories
    -I, --no-ignore         Do not respect .(git|fd)ignore files
    -s, --case-sensitive    Case-sensitive search (default: smart case)
    -i, --ignore-case       Case-insensitive search (default: smart case)
    -a, --absolute-path     Show absolute instead of relative paths
    -l, --list-details      Use a long listing format with file metadata

OPTIONS:
    -d, --max-depth <depth>            Set maximum search depth (default: none)
    -t, --type <filetype>...           Filter by type: file (f), directory (d), symlink (l),
                                       executable (x), empty (e), socket (s), pipe (p)
    -e, --extension <ext>...           Filter by file extension
    -x, --exec <cmd>                   Execute a command for each search result
    -X, --exec-batch <cmd>             Execute a command with all search results at once
    -E, --exclude <pattern>...         Exclude entries that match the given glob pattern
    -c, --color <when>                 When to use colors: never, *auto*, always
    -S, --size <size>...               Limit results based on the size of files.
        --changed-within <date|dur>    Filter by file modification time (newer than)
        --changed-before <date|dur>    Filter by file modification time (older than)
    -o, --owner <user:group>           Filter by owning user and/or group
```

