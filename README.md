## Bash Commands

### - [Less](https://linux.die.net/man/1/less)
```console
$ less file_2.txt
(Showing contents of file_2.txt)

- Press "q" to exit less
- Use "g" to get to the first line of the file
- Use "SHIFT G" to get to the last line of the file

------------------------------------------------------------------------------
$ less -N file_1.txt
(Showing contents of file_1.txt with line count
on the left side of the screen)

-------------------------------------------------------------------------------
$ less file_*.txt

- Less can open multiple files at once, and you can
  navigate through them using: ":p" (Previous) and
  ":n" (Next)

```

### - [Diff](https://linux.die.net/man/1/diff)
```console
$ diff -s file_1.txt file_2.txt
(Checking the difference of two given files)

- The flag "-s" OR "--report-identical-files", reports when two files are the same.

-------------------------------------------------------------------------------
Ex:
$ cat file_1.txt file_2.txt
Test
Change-me

Test
Changed-you
Added another line

$ diff -s file_1.txt file_2.txt
2c2,3
< Change-me
---
> Changed-you
> Added another line
```

<br>

---

<br>

## MISC

### - Linked Files / Symbolic Links
> Symbolic links - like shortcuts in **Windows**, and aliases in **MacOS** - provide mechanism for referring to another file. <br>
> Symbolic links can be easily identified by using `ls -l`, and by using the `file` command.
```console
$ ls -l /usr/bin/awk
("-l" directing ls to use long listing format)

lrwxrwxrwx 1 root root 21 Jun  3  2021 /usr/bin/awk -> /etc/alternatives/awk
                                                    ^^
- The arrow means that /usr/bin/awk is not a regular file, instead
  it is a symbolic link, that points to another file.
- In this case, /usr/bin/awk points to /etc/alternatives/awk

-------------------------------------------------------------------------------
$ file /usr/bin/awk
/usr/bin/awk: symbolic link to /etc/alternatives/awk
(Determine the type of a file using the commando "file")

-------------------------------------------------------------------------------
$ file /etc/alternatives/awk
/etc/alternatives/awk: symbolic link to /usr/bin/gawk

- A symbolic link to /usr/bin/gawk, which is an ELF executable file

$ file /usr/bin/gawk
/usr/bin/gawk: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV),
dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2,
BuildID[sha1]=b863ebf57d3cc33d8e7fed3be0e0b5d235489b46, for GNU/Linux 3.2.0

-------------------------------------------------------------------------------
$ readlink -m /usr/bin/awk
(Reading symbolic link of /usr/bin/awk)

- The flag "-m" follows every symlink(symbolical link) in every component,
  without requirements on components existance.
```

<br>

---

<br>

## String Manipulation

### - Brace Expansion `{ }`
- **Ex**: preamble{expansion}postscript

> The pattern takes the form of an unchanging `preamble`, followed by a variable `expansion` component, followed by an unchanging `postscript`. <br>
> The varying part of the pattern is enclosed by a pair of curly braces. <br>
> The constant part before the opening brace is called `preamble`, and the constant part trailing <br>
> after the closing brace is a `postscript`. <br>
```console
$ echo Street-{Rio_Novo,Vila_Bela,Benedito}-Brazil
Street-Rio_Novo-Brazil Street-Vila_Bela-Brazil Street-Benedito-Brazil

- This pattern has been expanded into three(3) different text strings.
- Each string has the same preamble at the begining, and the same post
  script at the end. The middle part of the string varies, and is
  present in the same order specified in the pattern.

-------------------------------------------------------------------------------
$ echo Street-{Rio_Novo,Vila_Bela,Benedito}
Street-Rio_Novo Street-Vila_Bela Street-Benedito

- Both the preamble and postscript are optional.
- Here, we expand a pattern with a preamble, but with no postscript

-------------------------------------------------------------------------------
$ echo {Rio_Novo,Vila_Bela,Benedito}-Brazil
Rio_Novo-Brazil Vila_Bela-Brazil Benedito-Brazil

- Here, we expand a pattern with a postscript, but with no preamble

-------------------------------------------------------------------------------
$ echo {Rio_Novo,Vila_Bela,Benedito}
Rio_Novo Vila_Bela Benedito

- Here, we expand a pattern with a no preamble and no postscript
```

```console
$ echo I love {Rio_Novo,Vila_Bela,Benedito}
I love Rio_Novo Vila_Bela Benedito

- In here, bash considers "I love" and "{Rio_Novo,Vila_Bela,Benedito}"
  to be separate tokens.
- As a result, the brace expansion is performed but "I love" is not
  considered a preamble to the brace expansion.
- If we close "I love" in quotes, bash will sees as two(2) tokens.
  1st- "I love"  2st- "{Rio_Novo,Vila_Bela,Benedito}".
-------------------------------------------------------------------------------

- And so, bash still doesn't consider "I love" being a preamble
  to the brace expansion.
- But we can fix that by placing the quoted string
  immediately adjecent to the brace expansion, like so:

$ echo "I love "{Rio_Novo,Vila_Bela,Benedito}.
I love Rio_Novo. I love Vila_Bela. I love Benedito.
```

<br>

> It can also contain a range of integers or characters using the operator `..`
```console
$ echo Number-{1..5}
Number-1 Number-2 Number-3 Number-4 Number-5

-------------------------------------------------------------------------------
$ echo {a..z}
a b c d e f g h i j k l m n o p q r s t u v w x y z

- We can also use a sequence of characters, with the ".." operator.

-------------------------------------------------------------------------------
$ echo a{A{1,2},B{3,4}}b
aA1b aA2b aB3b aB4b

- In the expansion, the preamble (lowercase a), and the
  postscirpt (lowercase b), are constant.
- In the variable part of the expression,
  the brace expression is a list of items separated by comma
  "A{1,2}" AND "B{3,4}".
- Bash starts with the first item ("A{1,2}"), and this is a brace expanssion as well,
  which expands to two(2) strings. Uppercase A1 and uppercase A2.
```

<br>

> Practical example:
```console
$ mkdir {2008..2017}-{01..12}

- With this single command, we are abble to create a list of directories for
  all of the months over a range of 10 years!
```

### - Command Substitution `$() // ${}`
> Command substitution allows us to use the output of a command as an argument of another command

> The `$` sign is a symbol with special significance to bash. <br>
> But there are other characters that also have special significance to bash. <br>
> These include: `$`, `!`, `&`, `\`, ` `(Space, which bash uses to delimit tokens)

- Note: The command substitution is very different from piping. <br>
  Piping, allows us to redirect the output of one command, to the standard input of another.
```console
$ echo $(ls /etc/X11)

- Command substitution causes this output to be used as the argument to echo
```
```console
$ echo ${#USER}
6

(echo environment variable $USER, that prints the current logged user,
and also counting the amount of caracters of this output "#")

- The value for $HOME, $USER, $SHELL, $PATH, $LOGNAME, and $MAIL
  are set according to the appropriate fields in the password entry(login).
  $ man login(1)
```
```console
- Example of a simple program that reads from standard input and counts
  the amount of characters a user puts in.

#!/usr/bin/bash

read -rp "Enter a string: " RESP_

LEN_=$(expr "$RESP_" : '.*')

echo "The length of the input string is: $LEN_"
```

<br>



