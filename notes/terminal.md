- [Text Processing](#text-processing)
    - [awk](#awk)
    - [cut](#cut)
    - [paste](#paste)
- [Numeric, textual and statistical operations](#numeric-textual-and-statistical-operations)
    - [datamash](#datamash)
- [Prettifying](#prettifying)
    - [Pretty json](#pretty-json)
    - [Viewing CSV files in terminal](#viewing-csv-files-in-terminal)
    - [Preserving colors when redirecting to file](#preserving-colors-when-redirecting-to-file)
- [Others](#others)
    - [Recursive download of folder contents using wget](#recursive-download-of-folder-contents-using-wget)
    - [sshfs in Mac](#sshfs-in-mac)
    - [Redirecting output to both stdout and file](#redirecting-output-to-both-stdout-and-file)
    - [Save HTML webpage as screenshot](#save-html-webpage-as-screenshot)


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

