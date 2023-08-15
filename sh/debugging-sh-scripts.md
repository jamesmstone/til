# Interactively debug a sh script

My common (ba)sh script setup is a bit like this:

```shell
# By default, in a pipeline of commands, the exit status of the pipeline is determined by
# the exit status of the last command in the pipeline. This changes that to fail if any command fails
set -o pipefail 

# stop on exit
set -o errexit 

# trace , print all commands before running.
set -x
```

Another approach that I just thought of that is useful is to enter an interactive shell should a command fail. eg

```shell

may_fail || bash
```

This is particularly useful when in a loop and subshell:

```shell

find "$root_directory" -type d | while read -r dir; do
    { 
      cd "$dir"  
      may_fail || bash # explore current state if this fails.
    }  
done
```

It allows the loop to pause until you're ready to continue by pressing <kbd>Ctrl</kbd> + <kbd>d</kbd>, or you can exit entirely using <kbd>Ctrl</kbd> + <kbd>c</kbd>.
