---
title: Bash Debugging
---
**Credit:** I learned all of these tips from [Julia Evans](https://jvns.ca/), via [this comic](https://wizardzines.com/comics/bash-debugging/) she wrote.

The following instructs `bash` to print out every line of a script as it executes, with all variables expanded.

```bash
#!/bin/bash
set -x
```

You can also run the script with the `-x` option for the same effect.

```shell
bash -x script.sh
```

To make the script run one line at a time, use the `DEBUG` signal, which is triggered before every line of code:

```bash
trap read DEBUG
```

Even fancier, put this at the start of your script to confirm every line before it runs:

```bash
trap '(read -p "[$BASH_SOURCE:$LINENO] $BASH_COMMAND")' DEBUG
```

### Error messages

This is a handy function for erroring out of a script:

```bash
die() { echo $1 >&2; exit 1; }

# example usage
# the program will exit and "oh no!" will print if `some_command` fails
some_command || die "oh no!"
```
