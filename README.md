<h2 align="center">Summary</h2>

- [Introduction](#introduction)
	- [What is Shell](#--what-is-shell-)
	- [What is Bash](#--what-is-bash-)
	- [SH = Bash?](#--sh--bash-)
	- [What is Dash in Debian](#--what-is-dash-in-debian-)
	- [How to execute a Bash program](#--how-to-execute-a-bash-program-)
	- [What is POSIX-Conformant](#--what-is-posix-conformant-)
	- [Builtin & Keyword](#--builtin--keyword)

- [Bash Commands](#bash-commands)
	- [awk](#--awk)
	- [cat](#--cat)
	- [chmod](#--chmod)
	- [cut](#--cut)
	- [compgen](#--compgen)
	- [diff](#--diff)
	- [dpkg](#--dpkg)
	- [less](#--less)
	- [ls](#--ls)
	- [nice / renice & intruduction to processes](#--nice--renice--introduction-to-processes)
 	- [parallel](#--parallel)
	- [paste](#--paste)
	- [seq](#--seq)
- [General](#general-back-to-top)
	- [File Permissions](#--file-permissions)
	- [Control Operators](#--control-operators)
	- [Redirection Operators](#--redirection-operators)
 		- Includes: **stdin**, **stdout**, **stderr**; **<**, **<>**, **>**, **>|**, **>>**, **>&**, **here strings**, **here documents**.
	- [Linked Files: Symbolic Links / Hard Links](#--linked-files--symbolic-links--hard-links-back-to-top)
	- [Shell Expansions / String Manipulation](#string-manipulation-back-to-top)
		- [Brace Expansion](#--brace-expansion--)
			- [Range Expansion](#--range-expansion-nn-nn-n-for-number-)
		- [ANSI-C Quoting](#--ansi-c-quoting-)
		- [Arithmetic Expansion](#--arithmetic-expansion-)
		- [Command Substitution & Parameter Expansion](#--command-substitution---parameter-expansion----back-to-top)
		- [Process Substitution](#--process-substitution---)
- [License](#license)

<!--

<p align="center">
  <a href="#bash-commands">CLI Commands</a> •
  <a href="#--control-operators">Control Operators</a> •
  <a href="#--linked-files--symbolic-links--hard-links">Linked Files</a> •
  <a href="#string-manipulation">String Manipulation</a> •
  <a href="#--redirection-operators-no-images-since-output-is-not-shownvisible">File Manipulation</a>
</p>

-->

<p align=center>
  <img align="center" src="https://img.shields.io/github/stars/RodricBr/Bash-Professional?color=00BC19&logo=apachespark&logoColor=D9E0EE&style=for-the-badge&labelColor=191c27">
  &nbsp;&nbsp;
  <img align="center" src="https://img.shields.io/github/last-commit/RodricBr/Bash-Professional?&logo=github&style=for-the-badge&color=00BC19&logoColor=D9E0EE&labelColor=191c27">
  &nbsp;&nbsp;
  <img align="center" src="https://img.shields.io/github/repo-size/RodricBr/Bash-Professional?color=00BC19&logo=hackthebox&logoColor=D9E0EE&style=for-the-badge&labelColor=191c27">
  <img src="./assets/tron.png">
</p>

---

<br>

## Introduction
- First of all, what is the meaning of this repository? <br>

In this repository, I'll create a way to simply the learning path to become a GNU/Linux Bash Professional, everything I learn, <br>
I see that I think is going to be usefull I add to this repository. First, we'll have an introduction of GNU/Linux Bash, so that we won't be lost throughout the elapse of this repository.

<br>

### - What is **Shell**? <br>

The Shell is a CLI (Command-Line Interface) program that processes and interprets kernel commands and outputs the results to the user. <br>
In the old days, it was the only user interface available on a UNIX-like systems, such as Linux. Nowadays, we have graphical user interfaces (GUIs) in addition to command line interfaces (CLIs) such as the shell.

Shell is a "[REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)" (Read, Eval, Print and Loop); <br>
Read = Reads the command given by the user <br>
Eval = Executes the command <br>
Print = Returns the results of the given command to the user <br>
Loop = And awaits for another input <br>

<!--If you're really interested in shell history, I'll suggest this repository that shows a [shell ancestry](https://github.com/marcpaq/shellancestry) "tree".-->

<br>

### - What is **Bash**? <br>

Bash is just an application, and its primary job is to run other applications (in the form of commands) that are installed <br>
on the same system. It reads commands from the user input or from a file of commands and executes them, usually by turning <br>
them into one or more system calls. It is usually not part of the kernel since the command interpreter is subject to changes. <br>
If you're really interested, I really recommend reading this whole article: [link](https://questionanswer.io/why-is-command-interpreter-separate-from-kernel/)

Bash is the GNU Project's shell-the Bourne Again SHell (BASH). This is an sh-compatible shell that incorporates useful <br>
features from the Korn shell (ksh) and the C shell (csh). It is intended to conform to the **IEEE POSIX P1003.2/ISO 9945.2 Shell** <br>
and Tools standard. It offers functional improvements over sh for both programming and interactive use. In addition, <br>
most sh scripts can be run by Bash without modification.

Bash supports a `--posix` option switch, which makes it more **[POSIX-compliant](#--what-is-posix-conformant-)**. It also tries to mimic **POSIX** if invoked as **sh**.
Bash also supports sh. Bash is a shell replacement for the Bourne shell. Bash is superset of sh. 

<br>

### - **SH** = **Bash**? <br>
For a long time, `/bin/sh` used to point to `/bin/bash` on most GNU/Linux systems.
As a result, it had almost become safe to ignore the difference between the two. But that started to change recently.

Some popular examples of systems where `/bin/sh` does not point to `/bin/bash` (and on some of which /bin/bash may not even exist) are:

- 1: Modern Debian and Ubuntu systems, which symlink sh to dash by default;
- 2: Busybox, which is usually run during the Linux system boot time as part of initramfs. It uses the ash shell implementation.
- 3: BSD systems, and in general any non-Linux systems. OpenBSD uses pdksh, a descendant of the KornShell. FreeBSD's sh is a descendant of the original Unix Bourne shell. Solaris has its own sh which for a long time was not POSIX-compliant; a free implementation is available from the Heirloom project.

How can you find out what `/bin/sh` points to on your system?

The complication is that `/bin/sh` could be a symbolic link or a hard link. If it's a symbolic link, a portable way to resolve it is:
```console
$ file -h /bin/sh
/bin/sh: symbolic link to bash
```

If it's a hard link, try: <br>
```console
$ find -L /bin -samefile /bin/sh
/bin/sh
/bin/bash
```
In fact, the `-L` flag covers both symlinks and hardlinks, but the disadvantage of this method is that it is not portable — POSIX does not require find to support the -samefile option, although both GNU find and FreeBSD find support it.

[More info](https://stackoverflow.com/questions/5725296/difference-between-sh-and-bash)

<br>

### - What is **Dash** in **Debian**? <br>

In GNU/Linux Debian, **Dash** was created to be the default `/bin/sh` for Debian. It is based on **ash** and is much faster than bash to interpret shell scripts/commands, up to 4x faster. However, it does not offer the practicalities of bash (autocomplete, aliasing, greater customization, autocompletion of various software, etc...)

To check if you are using **Bash** or **Dash**:
```console
$ echo $0
bash

OR

$ cut -d '' -f1 /proc/$$/cmdline
bash
```

<br>

### - How to execute a Bash **program**? <br>

To get started, you'll need to create a file using `touch`, `vim file.txt`... etc, using the `.bash` OR the `.sh` file extension. <br>
Although in Linux you don't need to specify the file extension, since Linux automatically identifies if the file is a simple text file, <br>
or if it is a bash/shell script file, we add the `.bash` OR `.sh` extension to the file, to make it more organized and if you're not using <br>
Linux.

Inside of the file you just created, the first thing we add is a **shebang** or a **hashbang**. It takes the form of a comment <br>
(lines starting with `#` are normally ignored by the shell), but the `!` right after the `#` indicates that it has to execute what is <br>
indicated that will process that script, in our case, we put a shebang calling the bash binary (`/bin/bash`), or maybe using a different <br>
method, that can be faster and easier, which I will explain below.

```console
#!/usr/bin/env bash
```
> The above example is running the **env** command with the **bash** parameter. The **env** serves to create a new environment, <br>
and the following parameter is the command that will be executed by the env in this environment. <br>
Since env uses the system path, bash will run without you having to define its exact path, making it faster than the default <br>

> **Important OBS**: From distro to distro, the location of **bash** or **python**, for example, can be different.

<br>
 
### - What is **POSIX-conformant**? <br>

POSIX is a set of standards defining how POSIX-compliant systems should work. Bash is not actually a POSIX compliant shell. In a scripting language we denote the interpreter as `#!/bin/bash` (shebang as explained above).

#### Analogy:

- Shell is like an interface or specifications or API.
- sh is a class which implements the Shell interface.
- Bash is a subclass of the sh.

<img src="https://i.stack.imgur.com/8Xvox.jpg">

In other words, Bash is **POSIX-conformant**, even where the POSIX specification differs from traditional sh behavior ([see Bash POSIX Mode](https://www.gnu.org/software/bash/manual/html_node/Bash-POSIX-Mode.html) for more info)

<br>

### - Builtin & Keyword

- Builtin: [about](https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Builtins.html) <br>
It is a command that has been implemented inside the shell for essential needs of the shell (`cd`, `pwd`, `eval`), or speed in general or to avoid conflicting interpretations of external utilities in some cases. <br>
Below are the list of builtins. (Or just execute `compgen -b` to show all builtins)

> **compgen** is a shell builtin. It displays possible completions depending on the options. <br>
> Intended to be used from within a shell function generating possible completions. <br>
> If the optional WORD argument is supplied, matches against WORD are generated. <br>

<details>
<summary>Expand to view all builtins</summary>

```bash
.
:
[
alias
bg
bind
break
builtin
caller
cd
command
compgen
complete
compopt
continue
declare
dirs
disown
echo
enable
eval
exec
exit
export
false
fc
fg
getopts
hash
help
history
jobs
kill
let
local
logout
mapfile
popd
printf
pushd
pwd
read
readarray
readonly
return
set
shift
shopt
source
suspend
test
times
trap
true
type
typeset
ulimit
umask
unalias
unset
wait
```

</details>

- Keyword: [about](https://www.gnu.org/software/bash/manual/html_node/Reserved-Word-Index.html) <br>
A keyword is also known as "reserved word". (Execute `LESS=+/"keyword" man bash` for more info) <br>
And **Reserved Words** are words that have a special meaning to the shell. <br>
The following words are recognized as reserved when unquoted and either the first word of a simple command or the third word of a case or for command. (Or just execute `compgen -k` to show all keywords)

<details>
<summary>Expand to view all keywords</summary>

```bash
if
then
else
elif
fi
case
esac
for
select
while
until
do
done
in
function
time
{
}
!
[[
]]
coproc
```

</details>

<br>

<hr>

## Bash Commands

> **Note**
>
> Use the command `info` before any command you'd like to get more information about.

### - [Awk](https://linux.die.net/man/1/awk)
> `awk` is a pattern scanning and processing language. <br>
> Awk is used as a command as often as it is used as an interpreted script, <br>
> and can be useful to write tiny but effective programs in the bash command line.

<img src="./assets/awk.png">

> A simple example of the useful usage of an **if statement** using `awk`. <br>

<img src="./assets/awk-if-statement.png">

<hr>

### - [Cat](https://linux.die.net/man/1/cat)
> Wouldn't be a complete repository without mentioning `cat`, the program which concatenates files and print them to the stdout. <br>
> `cat` is generally used (shouldn't be) to show the contents of any kind of files. Just like so: <br>
```console
$ cat script.sh
#!/usr/bin/bash

echo -e "Hello, World\n"
echo -e "\nThis is a loop:"
for ((X_ == 1; X_ <= 10; X_ ++)); do
  echo -e "$X_"
done
```

> In this simple example, I'm using the `-n` option flag to number all output lines, including blank lines. <br>
```console
$ cat -n script.sh
     1  #!/usr/bin/bash
     2
     3  echo -e "Hello, World\n"
     4  echo -e "\nThis is a loop:"
     5
     6  for ((X_ == 1; X_ <= 10; X_ ++)); do
     7    echo -e "$X_"
     8  done
```

> Using the `-b, --number-nonblank` option flag the output will number only nonempty output lines. <br>
```console
$ cat -b script.sh
     1  #!/usr/bin/bash
     
     2  echo -e "Hello, World\n"
     3  echo -e "\nThis is a loop:"
     
     4  for ((X_ == 1; X_ <= 10; X_ ++)); do
     5    echo -e "$X_"
     6  done
```

<hr>

### - [Chmod](https://linux.die.net/man/1/chmod)
> `chmod` is a command that allows users to change read and write permissions in UNIX systems.

```console
$ chmod +x programa.sh
- Changes the permission for "programa.sh" to executable, hence the "+x".
  The operator "+" causes the selected file mode bits to be added to the existing file mode bits of each file.
  
$ chmod -x programa.sh
- Removes the executable permission for "programa.sh".
  The operator "-" causes them to be removed.

$ chmod =x programa.sh
- Ignores all permissions, set them exactly as provided.
  The operator "=" causes them to be added and causes unmentioned bits to be removed except
  that a directory's unmentioned set user and group ID bits are not affected.
```

- See [File Permissions](#--file-permissions) for more information about the binary logic behind
- changing permissions using Octal Mode (numbers like `chmod 777`)

<hr>

### - [Cut](https://linux.die.net/man/1/cut)
> `cut` is a command which removes/replaces/selects sections from each line of given files.

```console
$ ls arq[1-4] | cut -d " " -f1
arq1
arq2
arq3
arq4

- OBS: Default output without using cut: arq1  arq2  arq3  arq4
- "-d" delimiter option flag considering space as a field separator or delimiter.
  So we'll cut the delimiter "space" from field/line 1. (Removes all spaces from the first line)
```

> Cutting from line N to N:
```console
$ seq 10 | cut -f3-5 -d$'\n'

- seq 10 will output numbers 1-10 on each 10 lines (I'm just simulating a file).
  cut with the option flag "-f" will select from fields 3 to 5 on the lines. (From line 3 to 5)
  and will add a line break delimiter using the "-d" option flag (used the ANSI-C Quoting to interpret the line break "\n").
```

<hr>

### - [Compgen](https://www.gnu.org/software/bash/manual/html_node/Programmable-Completion-Builtins.html)
> `compgen` is a bash builtin that generates possible completion matches for word according to the options, <br>
> which may be any option accepted by the complete builtin with the exception of -p and -r, and write the matches to the standard output.

```console
$ compgen -c
- Shows a list of all executable programs on your system.
  Using "wc -l" can help you count how many programs you have in total.

------------------------------------------------------------------------------
$ compgen -b
- Shows a list of all builtins in the system

------------------------------------------------------------------------------
$ compgen -v
- Shows a list of all variables defined in the system, counting those that you
  defined manually.

------------------------------------------------------------------------------
$ compgen -a
- Shows a list of all aliases defined in the system, counting those that you
  defined manually.

------------------------------------------------------------------------------
$ compgen -k
- Shows a list of all the keywords in the system

# Flags: "-abcdefgjksuv"
```
- A fun little example of how it could be useful to you:
<img src="./assets/banner-example.png" alt="banner example">

<hr>

### - [Diff](https://linux.die.net/man/1/diff)
> `diff` allows you to compare two files line by line.
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

- 2c(2 changes) 2,3(Line 2 and 3)

-------------------------------------------------------------------------------
- Another cool example, is using the -N, -a, -u and -r flags.
  Which shows a more simple way of understanding the differencies between
  the files, avoidinging incompatibilities and giving more information about them.

$ diff -Naur file_1.txt file_2.txt
--- file_1.txt  2022-11-07 14:34:03.870977100 -0300
+++ file_2.txt  2022-11-07 14:34:27.607286200 -0300
@@ -1,2 +1,3 @@
 Teste
-Change-me
+Changed-you
+Added another line
```

<hr>

### - [Dpkg](https://linux.die.net/man/1/dpkg)
> `dpkg` is a package manager for Debian, or Debian based distributions.

### - [Dpkg-reconfigure](https://manpages.ubuntu.com/manpages/bionic/man8/dpkg-reconfigure.8.html)
> `dpkg-reconfigure` is a command line tool used to reconfigure an already installed package. <br>
> In this example, I'll show how you can change your bash font using this command.
```console
$ sudo apt install fontconfig
- The command above, will install "fc", which allows us to check/list all of the fonts installed on our system,
  by russing: "fc-list".

------------------------------------------------------------------------------
$ sudo apt install xfonts-terminus # Instalar a fonte Terminus
- Installing the font "Terminus", from xfonts, which is the largest collection of fonts.

------------------------------------------------------------------------------
$ sudo dpkg-reconfigure -plow console-setup # Selecionar a fonte Terminus
- Using dpkg-reconfigure followed by the option flag "-plow".
  "-plow" flag shows all questions, irrespective of whatever default might have been set elsewhere.
- 1- Select the encoding type
  2- The character set to support (Latin1 is recommended)
  3- Select the font name. In this example, you'd have to select "Terminus" font.
  4- And finally, the desired font size.

------------------------------------------------------------------------------
$ cat /etc/default/console-setup
- And to check if everything is as expected, we can cat console-setup
  and look for "FONTFACE", with the name of our font inside quotes.
```

<hr>

### - [Less](https://linux.die.net/man/1/less)
> `less` allows you to view (**but not change**) the contents of a text file one screen at a time.
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

<hr>

### - [Ls](https://linux.die.net/man/1/ls)
> `ls` is used to list file or directory contents.

<img src="./assets/ls.png">

<hr>

### - [Nice](https://linux.die.net/man/1/nice) / [Renice](https://linux.die.net/man/1/renice) & Introduction to processes.
> `nice` is used to add modified scheduling priority to certain processes. <br>
> Runs a command with an adjusted niceness, which affects process scheduling. <br>
> Nicenesses range from **-20** (most favorable scheduling) to **19** (least favorable).

> We can check a process niceness by using `top/htop` and looking for the initials **NI**, <br>
> or by using `ps` with the flag `-eo uid,pid,ppid,pri,ni,cmd`, again checking for the **NI** initials. <br>

<img src="./assets/nice-ps.png">

#### - Types of Processes
In general, we can classify processes into the following types:

- **Conventional**: Normal system processes. <br>
The kernel gives these processes priority values from **100** to **139**, with **120** being the default. Being **100** means higher priority, that is, it will have more CPU usage time, greater time sharing. In this case **139** is the lowest priority, lowest time. <br>

- **Real-Time**: Processes that need to be executed immediately, not following process scheduling. <br>
These are usually internal kernel processes. These processes receive priorities from **1** to **99**, and it is common for it to be represented by the "rt" code, for real-time. <br>

#### - Priority (PRI) vs Nice (NI)

- The priority of a process is defined automatically and dynamically by the Linux kernel, assuming the values mentioned above.

- **NICE** is an attribute that allows the administrator or user to influence the priority of the process. <br>
When we use nice and renice commands to set this attribute, we are setting a NICE which will consequently impact the priority. <br>
By default, the NICE of a process is **0**.

```console
# Checking the priority of a process:
$ nice -n11 waitor.sh &
- Adding a niceness of 11 to the waitor.sh (throwing it to the background using "&").
  (waitor.sh was a quick .sh file I made for example)

$ cat /proc/<PID OR waitor.sh>/sched
- In here, we'll see a bunch of information about that process.
  But grepping for the word "prio" will return us the priority of that process, which in my case is 120.
  Remember that 120 is the default priority that the kernel gives.

------------------------------------------------------------------------------
$ renice 12 -p <PID>
- Adding an additional "12" of niceness to the given process.

$ grep prio /proc/<PID OR waitor.sh>/sched
- Notice that now our process is not +12 niceness.
  It used to be 120. And 120 + 12 = 132, that's our current priority.
  
$ sudo renice -12 -p <PID>
- Only root can increase the priority of a process, so we execute renice with sudo.

$ grep prio /proc/<PID OR waitor.sh>/sched
- Now our priority is 108. With a lower value, the process has a higher priority,
  a higher time-sharing in the CPU/s.

$ top
- Notice how the process that we're modifying now shows "PR" as "08",
  which is short for 180. And "NI"(nice) -12.

------------------------------------------------------------------------------
- If you couldn't find the sched file, try recursively grepping using the command:
$ grep -Hnri "prio" /proc/<PID>/
```

<hr>

### - [Parallel](https://www.gnu.org/software/parallel/man.html)
> `parallel` is a command to build and execute shell command lines from standard input in parallel.

> Showing the number of CPU cores my computer have. (This will be useful for how many cores we can use inside of a parallel command)
```console
$ nproc
4
```

- [Replacement Strings](https://www.gnu.org/software/parallel/man.html#options): <br>
> The parallel command contains a great amount of facilities that are generically called "Replacement String", and the main ones are the following: <br>
```console
{}   - Input Line (This replacement string will be replaced by a full line read from the input source. The input source is normally stdin (standard input), but can also be given with --arg-file, :::, or ::::.)
{#}  - Sequence number of the job to run / Sequential Process Number
{%}  - Job slot number / Core or CPU Number that is running the process
{.}  - Input line without extension
{/}  - Basename of input line (Functions just like the "basename" command / removes the directory name only leaving the file name)
{//} - Dirname of input line (Functions just like the "dirname" command / removes the file name only leaving the directory name)

There are more than that but for the purpose of this example I'll explain only those.
If you're interested in seeing all the replacement strings, go to: https://www.gnu.org/software/parallel/man.html#options
```
> Replacement strings are usually quoted, so special characters are not parsed by the shell. <br>
> The exception is if the command starts with a replacement string; then the string is not quoted. <br>

> Examples:
```console
$ seq 3 -1 1 | parallel sleep {} \; echo {}
1
2
3

- Notice that seq 3 -1 1 by itself generates numbers from 1 to 3 in a reverse manner (-1) (output == 321)
  But using parallel, it will cycle from sleep 3, to 2 and finally 1, then we use echo to show the output data.
  Instead of retreaving 321 it gives us 123 because the process that has "1" will finish first
  and be placed on top of the order, and so on with the following numbers (this show us that the command indeed executed in parallel)

- PS: The backslash before the semicolon ( \; ) is used because ";" is one of list operators (&&, ||) for separating shell commands.
  So to avoid special shell characters from interpretation, they need to be escaped with a backslash
  to remove any special meaning for the next character read and for line continuation.

------------------------------------------------------------------------------
$ seq 5 -1 1 | parallel sleep {} \; echo "{} \| {#} \| {%}"
2 | 4 | 4
3 | 3 | 3
1 | 5 | 4
4 | 2 | 2
5 | 1 | 1


- Notice that "seq 5 -1 1" is the equivalent to 54321.
  In this case we're doing the same as the example above, but instead, this time
  we'll be outputting the input line/data ( {} ); process number ( {#} ) and the Core/CPU number ( {%} ).

- Also notice that the pattern is not random. I'm not going to be able to explain exactly because
  it would be a mess, but looks for yourself and you'll understand.

------------------------------------------------------------------------------
$ parallel echo {.} ::: fake-directory/fake-file.extension
fake-directory/fake-file

- Removes the file extension

------------------------------------------------------------------------------
$ parallel echo {/} ::: fake-directory/fake-file.extension
fake-file.extension

- Works just like the "basename" command.

------------------------------------------------------------------------------
$ parallel echo {//} ::: fake-directory/fake-file.extension
fake-directory

- Works just like the "dirname" command.

More examples: https://manpages.ubuntu.com/manpages/xenial/man1/parallel_tutorial.1.html
```

> Using parallel to run `ping` command:
```console
$ parallel -j250 'timeout 2 ping -c 1 10.0.0.{1} >/dev/null && echo 10.0.0.{1}' ::: {1..255}
10.0.0.1
10.0.0.148
10.0.0.198

- In this example, we're using the "-j" parallel flag which limits
  the number of jobs that we can run at the same time.

- The command on which we'll execute in parallel is written between single quotes. (We could've used double quotes as well)
  The given command will ping to the local IPv4 address of 10.0.0.1 to 10.0.0.255
  with a 2 second timeout to check if the IPv4 is alive or not.
  Notice that I am enumarating the curly brackets to be later used by parallel
  as a Brace Expansion. (Ex: {1}, {2} --> ::: {a-z}, {0..9})
```

<hr>

### - [Paste](https://linux.die.net/man/1/paste)
> `paste` write lines consisting of the sequentially corresponding lines from each file, separated by TABs, to standard output. <br>
> With no FILE, or when FILE is -, read standard input. <br>

> Using paste without any arguments and only calling one file each time will simply display the contents of the determined file.
```console
$ paste numbers.txt
123
456

$ paste example.txt
12
345
6789
```

> Using paste while calling multiple files will print its contents side by side. Like so:
```console
$ paste numbers.txt example.txt
123     12
456     345
        6789
```

> We can also use paste with a delimiter to reuse characters from the user's given input instead of TABs that paste uses as default.
> The tabs will be turned into "+" characters.
```console
$ paste odd.txt even.txt -d+
1+2
3+4
5+6
7+8
9+10

------------------------------------------------------------------------------
$ paste odd.txt even.txt -d+ | bc
3
7
11
15
19

- Throwing the output from paste's command into bc (basic calculator)
```

> Adding a header to both columns using cat (right way of using cat, which concatenates files)
```console
$ echo -e "Odd\tEven" | cat - <(paste odd.txt even.txt)
Odd     Even
1       2
3       4
5       6
7       8
9       10

- Note: cat sees "-" as a filename, it treats it as a synonym for stdin.
  In simple terms, it receives data from primary/standard input (echo), and so cat concatanates odd.txt and even.txt files.
  
  IMPORTANT: All programs and utilities that are compiled with the "getopts" libbash library accepts "-" (dash) as the primary/standard input or output. 
  For more info about getopts lib: https://manpages.ubuntu.com/manpages/jammy/man3/getopts.3.html
```

> Pasting in serial with one file at a time instead of in parallel:
```console
$ paste -s odd.txt
1       3       5       7       9

$ paste -sd+ odd.txt
1+3+5+7+9

- Adding the delimiter flag (-d) alongside the serial flag (-s).

------------------------------------------------------------------------------
$ paste -sd+ odd.txt | bc
25

- Throwing the paste's output into bc.

------------------------------------------------------------------------------
$ ls arq[1-8] | paste -sd'\t\t\n'
arq1	arq2	arq3
arq4	arq5	arq6
arq7	arq8

- Paste's delimiter option flag can be composed of multiple delimiters.
```

> Creating a factorial using `seq` and `paste`:
```console
$ seq 4 | paste -sd\*
1*2*3*4

$ seq 4 | paste -sd\* | bc
24

- Throwing into bc

------------------------------------------------------------------------------
- We could shorten it using seq's own way to specify the delimiter:
  "-s" for separator.

$ seq -s\* 5
1*2*3*4
```

<hr>

### - [Seq](https://linux.die.net/man/1/seq)
> `seq` simply prints a sequence of numbers.

```console
$ seq 1 5
1
2
3
4
5

- Printing from 1 to 5.
------------------------------------------------------------------------------
$ seq 5 2 20
5
7
9
11
13
15
17
19

- Priting from 5 to 20 skipping 2 numbers each time.

------------------------------------------------------------------------------
$ seq -s " " 1 10
1 2 3 4 5 6 7 8 9 10

- Printing from 1 to 10 and using the "-s" separator option flag,
  which basically uses a string to separate numbers.
  
------------------------------------------------------------------------------
$ seq -ws " " 10
01 02 03 04 05 06 07 08 09 10

- Printing from 1 to 10 using the "-w" (equal width) which equalizes width by
  padding with leading zeroes separator flag. (1 in sequence of 100 turns into 001)

------------------------------------------------------------------------------
$ seq -f "Version: %02g" 2 4
Version: 02
Version: 03
Version: 04

- "-f" option flag is used to generate sequence in a formatted manner.
  "%02g" uses the format of the printf internal bash command. The "%02g" stands
  for use the output format: %g (which is the default) but with a 0 in front of the number.
  The leading 0 is used as padding only if necessary for printing lines 1-9 of the sequence.
  No 0 padding is necessary for printing lines 10-99 of the sequence.

- For example:

$ seq -f "Version: %05g" 2 4
Version: 00002
Version: 00003
Version: 00004

$ seq -f "Version: %01g" 2 4
Version: 2
Version: 3
Version: 4
```

<br>

<hr>

<br>


## General [(Back to Top)](#summary)

### - File Permissions

> Each file has a set of file mode bits that control the kinds of access that users have to that file. They can be represented either in symbolic form or as an octal number

- As seen from the `ls` command explanation:

<img src="./assets/ls.png">

```txt
-------------------------------
File permissions are 12 bits

              User  Group  All
0 0 0         110    110   100
| | V sticky  VVV    VVV   VVV
| V setgid    rwx    rwx   rwx
V setuid

For the r/w/x bits:
1 means "allowed" in binary
0 means "not allowed"

-------------------------------

110 in binary is 6

So rw- r-- r--
=  110 100 100
=   6   4   4

chmod 644 file.txt means change
the permissions to: rw- r-- r--
-------------------------------
```

- Using chmod to change permissions with binary and octal mode.
```console
Symbolic:  r-- -w- --x  |  421
Binary:    100 010 001  |  -------
Decimal:    4   2   1   |  000 = 0
                        |  001 = 1
Symbolic:  rwx r-x r-x  |  010 = 2
Binary:    111 101 101  |  011 = 3
Decimal:    7   5   5   |  100 = 4
           /   /   /    |  101 = 5
Owner  ---/   /   /     |  110 = 6
Group  ------/   /      |  111 = 7
Others ---------/       |  Binary to Octal chart

------------------------------------------------------------------------------
$ chmod 755 programa.sh
- Gives full permissions for the Owner and Read and Execute permission for Others
  7 == u=rwx (4+2+1 for user/owner)
  5 == g=rx (4+1 for group)
  5 == o=rx (4+1 for others)
```

<br>

<hr>

<br>

### - Control Operators
> A token that performs a control function. <br>
> It is a "newline" or one of the following: `||`, `&&`, `&`, `;`, `;;`, `;&`, `;;&`, `|`, `|&`, `(`, or `)`. (POSIX Definition) <br>

> A `!` is not a control operator but a [Reserved Word](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_04). It becomes a logical NOT [negation operator] inside Arithmetic Expressions and inside test constructs (while still requiring an space delimiter). <br>

#### 1.1 : List terminator <br>
- `;` : Will run one command after another has finished, irrespective of the outcome of the first. (List terminator)
<img src="./assets/c-operators-1.png">

> Here, I execute the `ping` command on a non-existent domain. <br>
> Even though the first command token failed(ping), the second command token will still be executed. <br>

- `&` : This will run a command in the background, allowing you to continue working in the same shell.
<img src="./assets/c-operators-2.png">

> Here, `ping` is launched in the background and `echo` starts running in the foreground immediately, without waiting for `ping` to exit. <br>

#### 1.2 : Logical operators <br>
- `&&` : Used to build **AND** lists, it allows you to run one command only if another exited successfully. <br>
<img src="./assets/c-operators-3.png">

> Here, `echo` will run after `ping` has finished and only if `ping` was successful. (if its exit code was 0) <br>
> Both commands are run in the foreground. <br>
> Exit Code: The value returned by a command to its caller. The value is restricted to eight bits, so the maximum value is 255.

> This command can also be written like so: `if command1; then command2; fi`. (if the return status is ignored)

- `||` : Used to build **OR** lists, it allows you to run one command only if another exited unsuccessfully. <br>
<img src="./assets/c-operators-4.png">

> Here, `echo` will only run if `listtt`(which is a unknown command) failed. (if it returns an exit status other than 0). <br>
> Both commands are run in the foreground. <br>

> This command can also be written like so: `if ! command1; then command2; fi`

- `!` : This is a reserved word which acts as the "**not**" operator (but must have a delimiter), used to negate the return status of a command — return 0 if the command returns a nonzero status, return 1 if it returns the status 0. Also a logical NOT for the test utility. <br>

```bash
! command1

[ ! a = a ]
```

#### 1.3 Pipe operator
- `|` : The pipe operator, passes the output of one command as input to another. A command built from the pipe operator is called a pipeline. <br>
<img src="./assets/c-operators-5.png">

> The pipe (as well as the Command Substitution) creates a **SubShell** to execute the commands, so use it only when it's really necessary. <br>
> When you create a SubShell, you are going to export the entire environment of your current shell to the subshell you are creating, <br>
> so it consumes a lot of time when used. However, most importantly, **anything done in a subshell is lost when the process terminates**. <br>
> When the child shell dies, it can't send back to the parent shell what you created in it. <br>

> Any output printed by `ls` is passed as input to `wc` (which will count words).

- `|&` : This is a shorthand for `2>&1` | in bash and zsh. It passes both standard output and standard error of one command as input to another.

```bash
command1 |& command2
```

#### 1.4 Other list punctuation
- `;;` : Used solely to mark the end of a case statement. Ksh, bash and zsh also support ;& to fall through to the next case and ;;& (not in ATT ksh) to go on and test subsequent cases.

- `(` and `)` are used to group commands and launch them in a subshell.
- `{` and `}` also group commands, but does not launch them in a subshell.

### - Redirection Operators
> In the shell command language, a token that performs a redirection function, is one of the following symbols: `<`, `>`, `>|`, `<<`, `>>`, `<&`, `>&`, `<<-`, `<>`.
> These allow you to control the input and output of your commands. They can appear anywhere within a simple command or may follow a command. <br>
> Redirections are processed in the order they appear, from left to right. <br>

> Redirection is a feature in Linux such that when executing a command, you can change the [standard input/output](https://www.gnu.org/software/gnuastro/manual/html_node/Input-output-options.html) devices.

<img src="./assets/exit-codes.png">

> Where every process receives three **File Descriptors**(FD) by default:
- stdin (0) - Exit Code refers to the **Standard Input** (data inserted to the program/process)
- stdout (1) - Exit Code refers to the **Standard Output** (data printed/outputed by the program/process)
- stderr (2) - Exit Code refers to the **Standard Error** (**from 2 - 255**, stands for messages of error outputed by a program/process)

> Here's a cool example of how file descriptors work: <br>

<img src="./assets/file-descriptor-1.png">

<br>

- `<` : Gives input to a command.
```bash
command < file.txt
```

> Instead of using `cat` to show the contents of a file and throw it's output to `wc -l` <br>
> we use `<` to give input to a command. Just like so:

<img src="./assets/do_not-do.png">

> And here's a performance difference between these two cases:

<img src="./assets/performance-diff.png">

> Execute command on the contents of file.txt

- `<>` : Same as above, but the file is open in **read+write** mode instead of **read-only**.

```bash
command <> file.txt
```

> If the `file.txt` doesn't exist, it will be created. That operator is rarely used because commands generally only read from their stdin.

- `>` : Directs the output of a command into a file.
```bash
command > out.txt
```

> Save the output of command as `out.txt`. If the file exists, its contents will be overwritten and if it doesn't exists, it will be created.
> This operator is also often used to choose whether something should be printed to standard error or standard output. Like so:
```bash
command >out.txt 2>error.txt
```

> `>` will redirect standard output and `2>` redirects standard error.
> Output can also be redirected using `1>` but, since this is the default, the "1" is usually omitted and it's written simply as `>`

> Concluding: to run `command` on `file.txt` and save its output in `out.txt` and any error messages in `error.txt` you would run:
```bash
command < file.txt > out.txt 2> error.txt
```

- `>|` : Does the same as `>`, but will overwrite the target, even if the shell has been configured to refuse overwriting. (with `set -C` OR `set -o noclobber`)
```bash
command >| out.txt
```

> If `out.txt` exists, the output of `command` will replace its content. If it does not exist it will be created.

- `>>` : Does the same as `>`, except that if the target file exists, the new data are **appended**.
```bash
command >> out.txt
```

- `>&` : (per POSIX spec) when surrounded by **digits** (`1>&2`) or `-` on the right side (`1>&-`) either redirects only one file descriptor or closes it (`>&-`).

> A `>&` followed by a file descriptor number is a portable way to redirect a file descriptor, and `>&-` is a portable way to close a file descriptor.


#### All the characters we saw receive/send data from/to files, but in addition to these, we also have: <br>

- `<<<` - Here Strings : Replaces the `echo SOMETHING | CMD` to `CMD <<< SOMETHING`. And by the way, that's exactly why **Here Strings** were created, they have more performance than using it with pipe (`|`).

<img src="./assets/here-strings.png">

> The **Here strings** (<<<) was created to avoid this type of construction that was very necessary. The problem is that the command after the pipe (`|`) is executed in a subshell and this fork, in addition to being slower, at the end destroys the entire environment created/modified by it.
> Here's an example of it.

<img src="./assets/here-strings-difference.png">

- `<<[-]` - Here Documents : Uses a form of [I/O redirection](https://tldp.org/LDP/abs/html/io-redirection.html#IOREDIRREF) to feed a command list to an interactive program or a command, such as **ftp**, **cat**, or the **ex** text editor. It is also known as a special-purpose code block. (The space after the double "less than" symbols is not necessary)
```
COMMAND <<InputComesFromHERE
...
...
...
InputComesFromHERE
```

> A **limit string** delineates (frames) the command list. The special symbol `<<` precedes the limit string. This has the effect of redirecting the output of a command block into the stdin of the program or command. It is similar to `interactive-program < command-file`, where **command-file* contains: <br>
```
command #1
command #2
...
```

> The here document equivalent looks like this: <br>
```
interactive-program <<LimitString
command #1
command #2
...
LimitString
```

- Practical example 1.0: Sends message to everyone logged in the terminal (using the `wall` command). <br>
```bash
#!/bin/bash

wall <<xxxxPizzaInformationxxxx
E-mail your noontime orders for pizza to the system administrator.
    (Add an extra dollar for anchovy or mushroom topping.)
# Additional message text goes here.
# Note: 'wall' prints comment lines.
xxxxPizzaInformationxxxx

exit
```

- Practical example 1.1: Adding content to an existing file using the command `ed`, which is a text editor. <br>
```bash
#!/bin/bash

filename="here-documents.txt"
ed -s "$filename" <<EndOfCommand
i
This file was created automatically from
a shell script using ed text editor.
.
w
EndOfCommand
```
> Notice how I'm using "**i**", "**.**" and "**w**" on the sequence. Those strings tells "ed" to first: start insert mode, then proceeds to write the contents, and writes/saves the file. <br>

- We could also do something similar using vim: <br>
```bash
#!/bin/bash

filename="here-documents.txt"

vim -c ":wq! $filename" - << EOF
This file was created automatically from
a shell script using vim text editor.
EOF
```

- Image example: <br>
<img src="./assets/here-documents.png">

<!--
https://unix.stackexchange.com/questions/159513/what-are-the-shells-control-and-redirection-operators
-->

<br>

<hr>

<br>

### - Linked Files / Symbolic Links && Hard Links [(Back to Top)](#summary)
> Symbolic links - like shortcuts in **Windows**, and aliases in **MacOS** - provide mechanism for referring to another file. <br>
> Symbolic links can be easily identified by using `ls -l`, and by using the `file` command. <br>

<br>

- Before we go any futher, we first need to understand what an `inode` is. [Full Explanation](https://www.slashroot.in/inode-and-its-structure-linux) <br>

> In general, an **inode**(index node) is an identification in the disk for every file and directory in the filesystem. <br>
> Whenever a user or a program needs access to a file, the operating system first searches for the exact and unique inode (inode number), <br>
> in a table called as an inode table. <br>
> Inodes do not store actual data. Instead, they store the metadata where you can find the storage blocks of each file’s data <br>
> and the metadata that contains in an inode are: <br>

- Mode: <br>
This keeps information about two things, one is the permission information, the other is the type of inode. <br>
For example, an inode can be of a file, directory or a block device etc. <br>

- Owner Info: <br>
Access details like owner of the file, group of the file etc. <br>

- Size: <br>
This location store the size of the file in terms of bytes. <br>

- Time Stamps: <br>
It stores the inode creation time, modification time, etc. <br>

> Now comes the important thing to understand about how a file is saved in a partition with the help of an inode.

- Block Size: <br>
Whenever a partition is formatted with a file system. It normally gets formatted with a default block size. <br>
Block size is the size of chunks in which data will be spread. So, if the block size is 4K, then for a file <br>
of 15K it will take 4 blocks(because 4K\*4 16), and so we waste 1K. <br>

<br>

> Hard Links - are additional pointers to an inode(inode and directory structures work together to provide an underpinning framework that stores all the metadata for every file and directory. filesystem ext4, ntfs... etc), meaning they can exist only on the same volume as the target. <br>
> Executing `ls -i` will tell you what inode you have for a determined file.

<img src="./assets/ls-i.png">

> When we use `ln` without any flags, he creates a hard link, that points to inode of a file. Therefore, he gets access to the "real" location of the file. <br> 
> If we deleted **teste2**, which was the first file created, he would still function as normal, since he's pointin to an inode and not the the file itself. <br>
<img src="./assets/no-flag-ln.png">

> In this example, it would be using the symbolic link. See that when you create the symbolic link, it doesn't show the original data of the file, and it still has an `l` before the permissions, showing that it's a symbolic link. <br>
> The inode of the two are different, since I referred that file, to the destination file, and not to its inode. <br>
> Finally, on the last line, it shows what happens when the file is deleted. It will simply "kill" the link. If I do a `cat`(command) on the link, it will say that the file was not found/does not exist, since the link reference is to the file, not to the inode.
<img src="./assets/symbolic-link.png">

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

<hr>

<br>

## String Manipulation [(Back to Top)](#summary)

### - Brace Expansion `{ }`
- **Ex**: preamble{expansion}postscript

> **OBS**: Brace Expansion does not starts with a `$`, only when we're dealing with **Parameter Expansion**. <br>

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
  A Token is a sequence of characters considered a single unit by the shell. It is either a word or an operator.
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

- In addition, we can use brace expansion to execute a command block without having to spawn a subshell. <br>
```console
$ { echo a; wc -l urls.txt; echo $BASHPID; }
a
9 urls.txt
8

- Notice that we have to use space in the beginning of the expansion
  between the command and the brace character ( {<SPACE><COMMAND> ).
```

<br>

#### - Range Expansion `{N..N}` `{N,N}` (**N** for number) <br>
> It can also contain a range of integers or characters using the operator `..` <br>
> In this case, we call it **Range Expansion** <br>

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
- In the variable part of the expresion,
  the brace expression is a list of items separated by comma
  "A{1,2}" AND "B{3,4}".
- Bash starts with the first item ("A{1,2}"), and this is a brace expansion as well,
  which expands to two(2) strings. Uppercase A1 and uppercase A2.
  
-------------------------------------------------------------------------------
$ echo {1..5}{0,5}%
10% 15% 20% 25% 30% 35% 40% 45% 50% 55%

- List of percentages that are going up by 5, we use one brace expansion
  as a prefix to another. And we also have "%" sign as a suffix.
OR (in bash 4 >)

$ echo {10..55..5}%
10% 15% 20% 25% 30% 35% 40% 45% 50% 55%

- From 10 to 55, skipping 5. More simple!
```

<br>

> Practical example:
```console
$ mkdir {2008..2017}-{01..12}

- With this single command, we are abble to create a list of directories for
  all of the months over a range of 10 years!
```

- Cool Brace Expansion example:
```console
$ man man
- Checking the manual for manual

$ man{,}
- Checking the manual for manual, if you have an empty element in your Brace Expansion,
  it doesn't add anything, but it adds a new word anyway.
```

<br>

<hr>

<br>

### - ANSI-C Quoting `$''`
> Causes escape sequences to be interpreted. <br>
> Using `$` as a prefix tells Bash to try to find a variable with that name. <br>
> `$'` is a special syntax (fully explained [here](https://www.gnu.org/software/bash/manual/html_node/ANSI_002dC-Quoting.html#ANSI_002dC-Quoting)) which enables **ANSI-C** string processing. <br>
> **Note:** ANSI-C Quoting is not POSIX.

```console
$ $'\167'
 18:52:39 up  1:26,  0 users,  load average: 0.52, 0.58, 0.59
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
- "167" is the UTF-8 octal for the ASCII "w". With backslash-escaped characters
  in string replaced as specified by the ANSI C standard.
  "w" is a command which shows who is logged on and what they are doing.

- I like to imagine ANSI-C Quoting as a medieval version of "echo -e", because...
$ echo -e "\0167"
w

- This time "w" doesn't get interpreted, because it's just a string.
  But to make it executable we can spawn a subshell, just like so:
$ $(echo -e '\0167')
 19:14:56 up  1:48,  0 users,  load average: 0.52, 0.58, 0.59
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT

-------------------------------------------------------------------------------
- Bonus cool epic example
$ echo 'I'$'\012am'''$'\012''Rodric'$'\041'''
I
am
Rodric!
```

> In this case, the single tick isn't "take value verbatim until the next single tick". It should be quite safe to use. The drawbacks are it's BASH only and quite uncommon, so many people will wonder what it means <br>

<br>

<hr>

<br>

### - Arithmetic Expansion `$(())`
> **Arithmetic Expansion** allows the evaluation of an arithmetic expression <br>
> and the substitution of the result. The format for arithmetic expansion is: <br>

```bash
$(( expression ))
```

The expression undergoes the same expansions as if it were within double quotes, but double quote characters in expression are not treated specially and are removed. All tokens in the expression undergo parameter and variable expansion, command substitution, and quote removal. The result is treated as the arithmetic expression to be evaluated. Arithmetic expansions may be nested.

- The evaluation is performed according to the rules listed in [here](https://www.gnu.org/software/bash/manual/html_node/Shell-Arithmetic.html). If the expression is invalid, Bash prints a message indicating failure to the standard error and no substitution occurs.

```console
$ cat parameter.sh
(($# == 0))&& { echo "Usage: ${0##/} parameter(s)" >&2; exit 1;} || { echo "Parameter count: $#";}

$ ./parameter.sh param1 Param2 PARAM3
Parameter count: 3

- Note how it starts counting by the second argument (1) instead of "0".
  Because we're using "$#" that prints the number of arguments, and the argument
  "0" is considered to be the program itself that is being executing.
```

<br>

<hr>

<br>

### - [Command Substitution](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html) `$()` & [Parameter Expansion](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Shell-Parameter-Expansion) `${} $() $(())` [(Back to Top)](#summary)
> Command substitution allows us to use the output of a command as an argument of another command

> The "`$`" character introduces **Parameter Expansion**, **Command Substitution**, or **Arithmetic Expansion**. <br>
> `()` just creates a compound command, running the commands inside the parentheses. `$()` does the same, but also substitutes the output. <br>

<img src="./assets/parameter-expansion1.png">

> The parameter name or symbol to be expanded may be enclosed in braces, which are optional but serve to protect <br>
the variable to be expanded from characters immediately following it which could be interpreted as part of the name. <br>

> When braces are used, the matching ending brace is the first "`}`" not escaped by a backslash or within a quoted string, <br>
and not within an embedded arithmetic expansion, command substitution, or parameter expansion. <br>

> But there are other characters that also have special significance to bash. <br>
> These include: `$`, `!`, `&`, `\`, ` `(Space, which bash uses to delimit tokens) <br>

- Note: The command substitution is very different from piping. <br>
  Piping, allows us to redirect the output of one command, to the standard input of another. <br>
  The Command Substitution creates a **SubShell** to execute the commands given.
```console
$ echo $(ls /etc/X11)

- Command substitution causes this output to be used as the argument to echo

-------------------------------------------------------------------------------
$ echo Greetings\ \&\ salutations\ {Rodric,Doom}\!
Greetings & salutations Rodric! Greetings & salutations Doom!

- Using space character "\" to escape literal space, since there is not usage
  of quoted string, and so bash considers it as a single token. Also proving
  that the "Greetings & salutations", which are the preamble of the
  brace expansion, still gets interpretated by bash.

```
```console
$ echo ${#USER}
6

(echo environment variable $USER, that prints the current logged user,
and also counting the amount of caracters of this output "#")

- The value for $HOME, $USER, $SHELL, $PATH, $LOGNAME, and $MAIL
  are set according to the appropriate fields in the password entry(login).
  $ man login(1)

-------------------------------------------------------------------------------
$ echo ${USER:0:3}
rod

(echo environment variable $USER, printing the first(0) character to(:)
the forth(3) character)

- Keeping in mind that the first character starts by "0" (0, 1, 2, 3, 4... etc)

```

- [Cool examples of Command Substitution](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html)

```console
- Example of a simple program that reads from standard input and counts
  the amount of characters a user puts in.

#!/usr/bin/bash

read -rp "Enter a string: " RESP_

LEN_=$(expr "$RESP_" : '.*')

echo "The length of the input string is: $LEN_"
```

> There are other special characters that retain their special meaning inside double quotes. <br>
> These exceptions includes: `$`, `${}`, `$()`, `$(())`, `\`(When used to escape special characters). <br>

<br>

<hr>

<br>

### - [Process Substitution](https://www.gnu.org/software/bash/manual/html_node/Process-Substitution.html) `<()` & `>()`
> Process Substitution allows a process's input or output to be referred to using a filename.

The `<(list)` syntax is supported by both, **bash** and **zsh**. It provides a way to pass the output of a command "`list`" to another command when using a pipe "`|`" is not possible.<br>

For example, when a command just doesn't support input from **stdin** or you need the output of multiple commands: <br>

```console
$ diff <(ls dirA) <(ls dirB)
```

`<(list)` connects the output of list with a file in `/dev/fd`, if supported by the system, otherwise a named pipe "`FIFO`" is used (which also depends on support by the system; neither manual says what happens if both mechanisms are not supported, presumably it aborts with an error). <br>

The name of the file is then passed as argument on the command line. <br>

- There are cases in bash where you can't use pipes: <br>

```console
$ some_command | some_other_command
```
> Because pipes introduce subshells for each component of the pipeline, when the subshells exits, any side-effects you were relying on would disappear. <br>

For example, this contrived example: <br>

```console
$ cat file | while read line; do ((count++)); done
$ echo $count
```
> Will display a blank line, because the "`$count`" variable does not exist in the current shell. <br>

A bash Process Substitution allows you to avoid this problem by allowing you to read from the "some_command" output like you would from a file.
Like so:
```console
$ while read line; do ((count++)); done < <(cat file)
# ....................................1.2
$ echo $count   # the variable *does* exist in the current shell
```

> Another cool example from one of the sections of this repository:
```console
$ echo -e "Odd\tEven" | cat - <(paste odd.txt even.txt)
Odd     Even
1       2
3       4
5       6
7       8
9       10
```

<br>

<hr>

<br>

# License
> This repository is under the MIT License. See [LICENSE](https://github.com/RodricBr/Bash-Professional/blob/main/LICENSE) file for more information.

<!--
# Ideas:
1- https://www.gnu.org/software/bash/manual/html_node/Definitions.html

2- https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html
-->
