Generations Radio Tools
=======================

Tools for [generations.fr](http://generations.fr/) radio.

Dump
----

### Description

This one-liner (splitted on multiple lines for readability) searches all
webradios from generations.fr, and scraps the website to dump for each
webradio:

1. the title,
1. the stream URL,
1. the XML file URL containing the last 10 songs.

### Dependencies

- `curl`
- `xmllint`
- `sed`, `awk`, `xargs`

### Examples

Simple run to get raw output:

```sh
generations-dump
```

Stream the output to format it as Markdown (used to generate `generations-dump.md`):

```sh
title='Generations Radios'
echo "$title" && echo "$title" | sed 's/./=/g' && echo

generations-dump | xargs -d'\n' -L3 sh -c \
    'echo "$0" && echo "$0" | sed "s/./-/g" && echo && echo "- Stream: $1" && echo "- Songs: $2" && echo'
```

List
----

### Description

List the titles from an XML file retrieved from the website API.

### Dependencies

- `xmllint`
- `sed`, `awk`

### Examples

Download the current funk titles and list XML data:

```sh
curl -s 'http://generations.fr/winradio/prog5.xml' | generations-list -
```

Same from a file:

```sh
generations-list prog5.xml
```
