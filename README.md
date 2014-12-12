generations.fr tools
====================

> Tools for [generations.fr](http://generations.fr/) radio.

generations-dump
----------------

### Description

This one-liner (splitted on multiple lines for readability) searches all
webradios from generations.fr, and scraps the website to dump for each
webradio:

1. the title,
1. the stream URL,
1. the XML file URL containing the last 10 songs.

### Dependencies

- curl
- xmllint

### Example

Simple run to get raw output:

```sh
generations-dump
```

Stream the output to format it as Markdown (used to generate
`generations-dump.md`):

```sh
title='Generations Radios'
echo "$title" && echo "$title" | sed 's/./=/g' && echo

generations-dump | xargs -d'\n' -L3 sh -c \
    'echo "$0" && echo "$0" | sed "s/./-/g" && echo && echo "- Stream: $1" && echo "- Songs: $2" && echo'
```

generations-list
----------------

### Description

List the titles from an XML file read from standard input, retrieved from
the website API.

### Example

Download the current funk titles and list XML data:

```sh
curl -s 'http://generations.fr/winradio/prog5.xml' | generations-list
```

generations-dump-all
--------------------

### Synopsis

```
generations-dump-all [-d DB] [-t TIME] URL
```

### Description

Infinite loop that will fetch `URL` every `TIME` seconds, append songs
using `generations-list` into `DB`, and remove duplicate entries from
this file.

The default database file will be named `db` in the current directory,
and the default time is 300 seconds (5 minutes).

I generated the `generations-funk.txt` file with this script.

### Example

```sh
readonly FUNK_URL='http://generations.fr/winradio/prog5.xml'

generations-dump-all "$FUNK_URL" # Fill `db` every 5 minutes
generations-dump-all -d mydb -t 600 "$FUNK_URL" # Fill `mydb` every 10 minutes
```
