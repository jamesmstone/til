# AWK from and to pattern matches

when using awk to parse a file I was aware it uses this basic structure:

```awk
pattern { action }
```

eg
```shell
seq 1 10 | awk '/2/{print $0}'
```
**Outputs**
```text
2
```

and if you omit an action it defaults to `print $0` ie print the whole matching line.

-----

I was not aware however you can have multiple `pattern` seperated by `,` and it will match from the first pattern until the last pattern


Relevant excerpt from the man page:

>A pattern may consist of two patterns separated by a comma; in this case, the action is performed for all lines from an occurrence of the first pattern though an occurrence of the second
> 
## Example

```shell
 seq 1 10 | awk '/2/,/8/'
```

**Outputs**
```text
2
3
4
5
6
7
8
```

Discovered from: https://www.reddit.com/r/bash/comments/12z9snv/comment/jhrlrg2/
