## Bash Commands

### - [Less](https://linux.die.net/man/1/less)
```bash
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

<br>

---

<br>

## MISC

### Linked files
```bash
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
                                                    
```
