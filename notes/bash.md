# Bash scripting
- [Bash scripting](#bash-scripting)
  - [Preferred shebang](#preferred-shebang)
  - [Safety first](#safety-first)
  - [getopts for parsing options and arguments](#getopts-for-parsing-options-and-arguments)

## Preferred shebang

```sh
#!/usr/bin/env bash
```

[Source](https://stackoverflow.com/a/10383546/3998252)


## Safety first

```sh
set -e   # exit on nonzero exit status
set -u   # abort on undefined variable
set -o pipefail  # don't hide errors within pipes
set -x    # print command before executing; useful for debugging

set -euo pipefail   # (unofficial) bash strict mode
```

Source [1](https://vaneyckt.io/posts/safer_bash_scripts_with_set_euxo_pipefail/), [2](http://redsymbol.net/articles/unofficial-bash-strict-mode/)


## getopts for parsing options and arguments

On Unix-like operating systems, `getopts` is a builtin command of the Bash shell. It parses command options and arguments, such as those passed to a shell script.

[Here is a good tutorial](https://www.computerhope.com/unix/bash/getopts.htm) on this topic. Example script from this tutorial.

```sh
#!/bin/bash
NAME=""                                        # Name of person to greet.
TIMES=1                                        # Number of greetings to give.
usage() {                                      # Function: Print a help message.
  echo "Usage: $0 [ -n NAME ] [ -t TIMES ]" 1>&2
}
exit_abnormal() {                              # Function: Exit with error.
  usage
  exit 1
}
while getopts ":n:t:" options; do              # Loop: Get the next option;
                                               # use silent error checking;
                                               # options n and t take arguments.
  case "${options}" in                         #
    n)                                         # If the option is n,
      NAME=${OPTARG}                           # set $NAME to specified value.
      ;;
    t)                                         # If the option is t,
      TIMES=${OPTARG}                          # Set $TIMES to specified value.
      re_isanum='^[0-9]+$'                     # Regex: match whole numbers only
      if ! [[ $TIMES =~ $re_isanum ]] ; then   # if $TIMES not a whole number:
        echo "Error: TIMES must be a positive, whole number."
        exit_abnormally
        exit 1
      elif [ $TIMES -eq "0" ]; then            # If it's zero:
        echo "Error: TIMES must be greater than zero."
        exit_abnormal                          # Exit abnormally.
      fi
      ;;
    :)                                         # If expected argument omitted:
      echo "Error: -${OPTARG} requires an argument."
      exit_abnormal                            # Exit abnormally.
      ;;
    *)                                         # If unknown (any other) option:
      exit_abnormal                            # Exit abnormally.
      ;;
  esac
done
if [ "$NAME" = "" ]; then                      # If $NAME is an empty string,
  STRING="Hi!"                                 # our greeting is just "Hi!"
else                                           # Otherwise,
  STRING="Hi, $NAME!"                          # it is "Hi, (name)!"
fi
COUNT=1                                        # A counter.
while [ $COUNT -le $TIMES ]; do                # While counter is less than
                                               # or equal to $TIMES,
  echo $STRING                                 # print a greeting,
  let COUNT+=1                                 # then increment the counter.
done
exit 0                                         # Exit normally.
```
