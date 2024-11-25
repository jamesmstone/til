# Find sqlite dbs

Lots of applications use sqlite dbs. It can be useful to try and find them all. For this you can use the following command

```
find . -type f -exec sh -c 'head -c 16 "$1" 2>/dev/null | grep -q "^SQLite format" && echo "$1"' _ {} \;
```

as all sqlite files start with the header string: "SQLite format 3\000" , see: [Database File Format](https://www.sqlite.org/fileformat.html)

it can then be helpful to get the size of each result

```
find . -type f -exec sh -c 'head -c 16 "$1" 2>/dev/null | grep -q "^SQLite format" && echo "$1 $(stat -c%s "$1" | numfmt --to=iec)"' _ {} \;
```