# SOME LINUX COMMANDS

## streams

- you have 3

1. stdin > /dev/stdin > 0
2. stdout > /dev/stdout > 1
3. stderr > /dev/stderr > 2

```sh
cat 0<tmnt.txt 1>/dev/stdout 2>&1 # take tmnt from stdin and then move the output to stdout

ls -z 2>/dev/null # redirect the error to nowhere
ls -z &>/dev/null # redirect stdout, stderr to nowhere

tac tmnt.txt # reverse order for cat
```

### tee

- it's like writing to stdout and the file at the same time
- if you use it for files it will empty them
- use more for commands
- it's used for when you want to use sudo with `>`

```sh
echo "dpkg bla bla serious stuff" | sudo tee /root-file # overwrites by default
echo "dpkg bla bla serious stuff" | sudo tee -a /root-file # appends
```

---

## Basics

```sh
last # login access log
lastb # checks bad login attempts

whoami # print user
env # print env vars
man # ual
clear
[] # optional
pwd # print working directory
ls --all --human-readable --size -S
cd # is posix
mkdir -p /all/new/stuff/not/there
touch woman apple.txt misterman.md bruh.py some-other-file.js
rm --verbose
rmdir # for empty dirs == rm -d
rm --recursive --force --interactive
xdg-open # is just like macs open
mv -t /destination/dir # moves file from destination to current directory
mv --update # move only when src file is newer than dest file
cp -r --preserve`
cp -s # makes symbolic links
```

---

## Difference between sudo su - and sudo -i and sudo -s

- `sudo -s`: gives you an elevated shell. but doesn't simulate a login: your env vars won't be root's. your workdir won't be `/root`
- `sudo -i`: **THE BEST LOGIN SIM**. gives you the root workdir and env vars.
- `sudo su -`: similar to -i but it mixes and matches the env vars which is not clean.

---

## Linking

```sh
ln -r # makes a hard link given a relative path, it has to be made _RELATIVE TO THE SYMLINK LOCATION NOT THE CURRENT DIRECTORY LOCATION_
```

- when you delete/update the source of the hard link, the hard link stays as is
- symlinks `ln -s` are vice versa

```sh
readlink -f # gets the location of the original source of link
```

---

## head - tail

```sh
head # prints first 10 lines in a file
head -n 5 # first 5
tail -n 5 # last 5 lines

tail -F # keeps monitoring the file for changes _perfect for logs_

date # shows date
```

---

## Piping

1. input pipes

```sh
< # instead of catting the file

<< # here doc, pass input data to a cmd till a specified ending text is seen, and then perform analysis on all previous input data. use it in bash scripts
cat << EOF # take the input and stop after EOF
some text bla bla
multi line stuff
EOF # multi line strings. BE CAREFUL WHEN COUNTING CUZ IT READS EOF

cat <<< "some string" # here string, instead of entering docs like heredoc, it is similar to echo "string" | cmd
```

2. output pipes

```sh
&> # pipes both stdout and stderr
> # writes to file. doesn't append. automatically creates
>> # appends to file
| # pipes the output to another command

command -v htop &> /dev/null # checks if htop exists and sends it's stdout & stderr to /dev/null
command >stdout.txt 2>stderr.txt
```

---

## Printing Files

```sh
cat -n file1 file2 # prints the entire files & concatenates them, prints line numbers (works great with grep)

less # > similar to cat but interactive

echo # prints text

wc -c -m -l -w # prints bytes count (c), char count (m), line count (l), word count (w)
cat log-file.log | wc -l > line-count.txt # counts log lines, writes it to another thing
```

---

## Sorting & Uniqueness

### sort

```sh
sort # just compares ascii stuff
sort --ignore-leading-blanks --dictionary-order --month-sort --numeric-sort --reverse
sort -nr # reverse num sort
sort -u # ignores duplicates
sort -u # ignores duplicates
sort -u # ignores duplicates
```

### uniq

```sh
uniq # prints the file with removed duplicates
uniq -c # adds duplicates count to lines
uniq -d # only prints duplicate lines
uniq -cd # prints duplicate lines and their count on the left
uniq -u # only prints unique lines
```

### example

```sh
sort general-commands/cmds.md | uniq -c | sort -nr | less # sorts lines, counts dups, sort by dups number in reverse (asec)
```

---

## Expansions & env vars

```sh
~ # home dir
```

### $env vars

- \*\*always wrap them with `""` when invoking

```sh
$? # show exit code of last cmd
$PATH # shows directories added to path
$SHELL # shows active shell location
$USER # same as whoami
$PWD # same as pwd
echo "/usr/some/commands/dir">> $PATH; source .bashrc # adds commands directory to path, reloads shell
```

### wild cards

```sh
* # same as regex, 0 or more
? # any single char, like regex (`.`)
*.???? # any file with 4 trailing chars after file extension
{} # array expansion. any char in the array separated by comas
{anton,app,bruh}.?? # any files named those 3, and follow by a 2 char extension
*.{py,js,md} # any file with py js or md extensions
touch Day{1..365}.day # creates files Day1.day etc...
```

---

## diff

```sh
diff 1.txt 2.txt # just shows the difference
diff -u 1.txt 2.txt # much more readable, just like git
```

---

## The finders

### find

```sh
find . # returns all files recursively
find . -type d # returns all dirs recursively
find . -type f # returns all dirs recursively
find . -name '*.js' # all files ending with js recursive, **exact matches**
find . -name '*E*' -or -iname 'F*' # returns all files containing E or starting with (F or f (case insensitive))
find . -name '*.md' -not -path '/some/excluded/path' # returns all files containing E or F or f
find . -name '*.md' -type f -size +100k -size -1M # any md file with size <1 mega >100 kilobyte
find . -name '*.md' -type f -mtime -2 # any md file modified in 2 days ago
find . -name '*.md' -type f -mmin -3 -delete # any md file modified 3 mins ago, then delete them
find . -type f -exec cat {} \; # executes cat on each output file, `{}` is a place-holder, `\;` stops executing
find . -type d -exec ls {} \; # ls all dirs
```

### grep

```sh
grep 'sometexttobefound' file.txt # you know, **case-sensitive**
grep -q # quiet mode, useful for bash scripts
grep -n -i 'sometexttobefound' file.txt # you know, with line numbers, make search case-insensitive
grep -n -C 2 'sometexttobefound' file.txt # C for context, add 2 lines before and after
grep -v 'some text' file.md # inverts the results, print lines not containing shit
```

---

## Storage stuff

### du - for dirs/files

```sh
du -g . # size of dir in giga
du -m * # size of each dir individually, in mega
du -ha # human readable size, lists each file in dirs too
du -h | sort -nr | head -n 5 # sort by size, get top 5
```

### df - for disk usgae

```sh
df -h # human readable
df /usr/bin # shows how much the dir takes from storage
```

---

## History

### history

```sh
history # history with nums, usually 500
! # run last cmd
!! # run last 2
!!! # you know
!143 # run cmd no. 143 in history
```

---

## Zipping

### gzip - gunzip

```sh
gzip a.md # zips a file, **deletes original**
gzip -c a.md > dest.gz # zips file, keeps it, outputs to stdout, so you pipe it
gzip -k b.md a.md # same as above
```

compression lvls (1-9) slower,better. 6 is default

```sh
gzip -1 -c a.md > a1.gz # compression lvl 1
gzip -r ./dirs # recursively dirs
gzip -kv a.md # shows compression progress
gzip -d a.gz # **decompress**, remove zip

gunzip -c a.gz > a.md # basically gzip -d
```

### tar - makes an archive

**IT'S NOT COMPRESSED BY DEFAULT YOU HAVE TO USE -z**

```sh
tar -czf archive.tar *.md # -c creates **compressed** tar, -f writes to archive
tar -cf archive.tar *.md # -c creates **uncompressed** tar, -f writes to archive
tar -xf archive.tar -C ./extracted # extracts all from archive
tar -tf archive.tar # lists files in tar
```

---

## alias

```sh
alias ll='ls -al' # you know

**IMPORTANT**
alias lsthis="ls $PWD" # `""`: variable is resolved @ declaration time, will ls the directory where you declared the alias
alias lscurrent='ls $PWD' # `''`: variable is resolved @ invocation time, ls current dir normally
```

## command

- runs commands bypassing any aliases

```sh
alias ll="ls -lahG"
command ll # cmd not found
command -v ls # location of ls
```

---

## xargs

- when you pipe commands, it moves to stdin
- some cmds accept input from stdin, like cat, less, wc
- but some dont
- so you pipe to xargs, and it translates the stdin in to cmd args

```sh
echo "/ $HOME" | xargs ls # ls every dir
ls ./dir/with/files/to/del | xargs -p rm # -p is interactive, to confirm action on all iterations
cat delete-files.txt | xargs -p -n1 rm # -n1 performs one iteration at a time

**IMPORTANT**
cat general-commands/todel.txt| xargs -p -I % sh -c 'du %; rm -i %;' # -I replaces a placeholder with the output.
```

---

## Users & Auth

### who

```sh
who # lists all logged in users, and when they logged in
who -aH # more info with table display
```

### su

```sh
su [username] # run the shell as another user
su -s /bin/zsh -c 'echo $SHELL' # run zsh as root and shows shell
```

### sudo

```sh
sudo -i # run current shell as root
sudo -u some_user cmd # run cmd as some_user with root privileges
```

### passwd

```sh
passwd # change your pass, prompts for old & new pass
sudo passwd <user> <new_pass> # change user pass with no pwd restrictions, no prompts for new pass
```

### chown - chgrp

```sh
chown mina file.txt # owner is mina
chown -R mina:user_group some/dir # recursive ownership to mina of user_group
chgrp mygroup file.txt # change ownership to mygroup
```

### understand your perms

- `-` file, `d` dir, `l` link
- `owner_user|owner_group|others`
- `read|write|xcute`
- `4|2|1` add them for combinations
- `(a)(ugo)(+-)(rwx)(124)(367)`

### chmod

```sh
chmod 623 file # = chmod u+rw g+w o+wx file
```

---

## systemd

### systemctl

```sh
systemctl # shows all bg processes
systemctl list-unit-files --type=service # shows all service units, much better looking

systemctl status apache2.service # apache status (loaded?, runtime,PID, DOCS, mem cpu usage, logs), some logs need sudo to show
systemctl enable --now apache2 # enables on boot, starts
systemctl disable apache2@mina # disables apache2 run by user mina
systemctl start
systemctl stop
systemctl disable

systemctl reload # doesn't go down, much perfered
systemctl restart # goes down then up, some units need it after changing unit file to apply changes
```

### unit files

- All unit files reside on `/usr/lib/systemd/system`

```sh
sudo nvim /usr/lib/systemd/system/apache2.service # edit unit file
```

- unit file config:
  - `After=` >> always run unit after this
  - `User=` >> the user that should run the unit (mostly %i which means the current user)
  - `ExecStart=` >> cmd that runs when unit starts
  - `ExecStop=` >> cmd that runs when unit stops
  - `WantedBy=`
    - `multi-user.target` >> it's a bg cmd that runs on terminal
    - `graphical.target` >> it's a gui app

### journalctl

```sh
journalctl -u nginx.service # nginx unit logs
journalctl -f -u nginx.service # follows the logs like tail -f
journalctl --since "2020-12-28 10:24:00" --until "2020-12-30 12:24:00" -u nginx.service
```

- you can use `/` to search inside `journalctl` log

---

## Text Manipulation commands

### read

```sh
read # reads from std, stores in $REPLY by default
echo $REPLY

read -s password # secret, doesn't show input you type
echo "$password"

read -p "Enter some input: " $input_var

read -n 5 -p "Only enter 5 chars in this prompt: " five_char_var
```

### fmt

```sh
fmt -w 20 file.txt # outputs file to be 20 char long, to fit small terminals
fmt -u file.txt # adds a space after each word, 2 spaces after each sentence
fmt -t # indents first line of paragraph
fmt -s # splits longer lines
```

### nl

```sh
nl file.txt # basically cat -n
```

### awk

- scripting lang to manipulate text
- takes from stdin, write to stdout

- uses **spaces** as delimiters by default

```sh
awk '{print}' tmnt.txt # prints all fields like cat

ls -l | awk '{print $2}' # second field from piped ls

awk '{print $1,$3}' tmnt.txt # print 1st & 3rd fields
leonardo leader
raphael hothead
michelangelo party-animal
donatello geek
```

- `$0` >> all fields
- `$NF` >> last field

```sh
# search for users, their home folders and their shells
awk -F ':' '{print $1, $6, $NF}' /etc/passwd # changed delimiter to :
awk -F ':' '{print $1" || "$6" || "$NF }' /etc/passwd # added some spacing
awk 'BEGIN{FS=":"; OFS="-"} {print $1, $6, $NF }' /etc/passwd # specify separator, then change it
```

- to use awk with a pattern, use `'/<add you regex pattern here>/'` :

```sh
# list all the unique shells on the system
awk -F "/" '/^\// {print $NF}' /etc/shells | uniq | sort # we looked for lines starting with a /, we escaped it cuz the patter itself uses /
```

- awk can have simple maths

```sh
df | awk '/\/dev\/sda/ {print $1 "\t" $2+$3*2 }'
# in df, get lines containing /dev/sda (/ is escaped)
# then add tab char as spacing
# then get the sum of $2 + $3*2
```

- you can filter by length

```sh
awk 'length($0) > 9' /etc/shells
# split by space, look for lines longer than 9 chars, print them all
```

- if statements exist

```sh
ps -ef | awk '{if($NF == "/usr/bin/zsh") print $0}'
# output of ps, split by space
# if last field is bin zsh then print all fields
```

- you have for loops too

```sh
awk 'BEGIN { for(i=1; i<=10; i++) print "The square root of", i, "is", i*i;}'
# try it
```

- more regex action
- `~` is like sql, to match regex stuff

```sh
awk '$1 ~ /^[l,d]/ {print $0}' tmnt.txt
```

### sed

- changes text in files, quickly
- writes to stdout, so write to file

1. `s` for substitute, followed by delimiter
2. **Always Use / As Delimiter Cuz Convention Till You Want To Replace the character itself**, then use another delimiter
3. word to replace, what to replace it with
4. then end with the delimiter
5. and `g` for global matching

```sh
sed 's/Pineapple/Feta/' tmnt.txt # s for substitute, replace Pineapple with Feta on the first match only
sed 's/Pineapple/Feta/g' tmnt.txt # s for substitute, replace Pineapple with Feta on all matches
sed 's/Pineapple/d' tmnt.txt > tmnt-edit.txt # wrote to new file, only first match DELETED

sed -i 's:/:|:g' tmnt.txt # writes to file in place, changed delimiter cuz i wanna change /

sed -e 's:usr:u:g' -e 's:bin:b:g' < /etc/shells
```

### cut - similar to `str[start:end]`

```sh
echo "bruh man yo" | cut -c 1-6 # only chars 1 to 6
echo "bruh man yo" | cut -c 11- # cuts last chars after the 11th char
```

### tr - poor man's sed

- **IMPORTANT** it looks for chars like regex, not find and replace like sed

```sh
echo "bruh man yo" | tr 'u' 'U' # replace u with U
echo "bruh man yo" | tr 'aeiou' 'AEIOU' # replace vowels with their CAPS
echo "bruh man yo" | tr -d 'aeiou' # delete all vowels

echo "bruuuuh" | tr -s 'u' # squeeze all u's into 1 u
```

---

## strace - ltrace

### strace - monitor program sys calls

- calls to the OS

```sh
man syscalls # all systemcalls
strace ssh # trace ssh sys calls
strace -p 134 # trace PID 134 sys calls
```

### ltrace - monitor program lib calls

```sh
ltrace ssh # trace ssh lib calls
ltrace -p 134 # trace PID 134 lib calls
```

---

## uname

```sh
uname
Linux
uname -a # all info
"Linux ub-vbox 5.15.0-67-generic #74-Ubuntu SMP Wed Feb 22 14:14:39 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux"
```

---

## Processes

### ps

```sh
ps # shows pids by the current user
ps ax # shows all pids, a for all users, x for all terms, cmds are cut
ps axww # show full cmds
ps axww | grep zsh # show full cmds
```

statuses (mostly 1 letter, 2 is more info):

- `I` a process that is idle (sleeping for longer than about 20 seconds)
- `R` a runnable process
- `S` a process that is sleeping for less than about 20 seconds
- `T` a stopped process
- `U` a process in uninterruptible wait
- `Z` a dead process (a zombie) + is a foreground pid
- `s` session leader

### top - interactive

```sh
top -o mem # sort by mem usage
```

### kill

```sh
kill 1424 # kill pid 1424
```

```sh
kill -HUP <PID> # hang up, tell process to close -1
kill -INT <PID> # interrupt, just like ctrl c, terminates -2
kill -KILL <PID> # tell the kernel to stop, terminate the proc -9
kill -TERM <PID> # tell the process to terminate itself -15
kill -CONT <PID> # tell the proc to continue -18
kill -STOP <PID> # tells the kernel to stop but not terminate the proc -15
```

### killall - kill but for all instances of the procces not just a pid

```sh
killall -HUP top
```

### jobs - fg - bg

```sh
command & # runs command in the background, gets stored in jobs
jobs # lists jobs
fg job_num # sends job to fg
bg job_num # sends job to bg
```

---

## Monitoring Misc cmds

### lsof - ls open files

- asks kernel to see which files are open, and what processes are using a file

```sh
lsof # pid, cmd, path of files open
lsof /path/to/open/file # what program is running the file

lsof -p 138 # all files open by PID 138
lsof -p 138 | grep .so # what shared libs is the app using
lsof -p 138 | grep log # what log files

lsof -u mina # what's open by mina

sudo lsof -i :80 # what's listening on port 80
sudo lsof -i tcp
```

### nmon

- has lots of modular stat commands

### iostat

- for disk usage

```sh
iostat 1 111 # refreshes every sec
```

### dstat

- for cpu usage

### vmstat

- for virtual memory
  `-d` for disk stats
  `-D` for disk stats table
  `-p sda1` for stats on a partition

### sar - sys activity recorder

- to enable monitoring, edit `/etc/default/sysstat`
- automatically logs every 10 mins using a cron job

---

## Scheduling

- always run commands as their full paths when writing scripts
- better security
- shells and distros change

### at

- has to be in US time format (24:00 08142020)
- executes commands with `/bin/sh`

```sh
at 21:04 08152023 -f path/to/scripy # to sched scripts
atq # to list the job queue
atrm job_num_1 job_num_2
```

### cron jobs

```sh
crontab #? shows crontab
crontab -e #? edits crontab, if you ran as user then it will be only yours
sudo crontab -u user -e #? cronjob for another user
```

- how to write cron jobs:
  ```sh
  30 1 * * 5 /usr/local/bin/script #? run this script every friday at 1:30
  m  h day_of_mth month day_of_week
  ```
  - specify what time it will run
  - the more specific the time, the less the cronjob will run
  - what months or days it will run
  - what days of the week it will run
  - sunday is both 0 & 7

---

## Internet

### wget

```sh
wget https://link.com/file.zip # download zip
wget -O my-file.zip https://link.com/file.zip # change name ONLY
wget -P /my/special/dir https://link.com/file.zip # change location ONLY
```

- wget automatically pauses the download if inturrepted

  ```sh
  wget -c /same/link/ # resume download
  ```

- it can install multiple files from a download file

```sh
wget -i files-to-download.txt
```

### curl

- more used for scripts
- outputs to stdout

```sh
curl link # outputs link
curl -v https://google.com # verbose mode, perfect for analyzing traffic
curl -i # shows resp headers alongside output
curl -I # shows only resp headers
curl -O myzip.zip www.some/link.zip # downloads file
curl --limit-rate 500K # sims dw speed
curl --data "title=hi&body=yo" # adds data, POST by default
curl --data -X PUT "title=hi&body=yo" # adds data, uses PUT
curl -u username:passwd # adds auth
curl -L # follows redirection
```

---

## Networking Commands

### know your public ip

```sh
curl icanhazip.com
```

### ufw

```sh
ufw status numbered
ufw default allow outgoing
ufw default deny incoming
ufw allow ssh # 22 by default. by default allow allows access from anywhere
ufw allow https/tcp # 80 by default
ufw enable
ufw delete 3 # delete rule 3 (from numbered)

ufw allow from your_public_ip to any port 22 proto tcp # only allows ssh (ssh uses tcp) from a public ip
```

### iptables

- built in linux
- no need to install ufw

```sh

```

### traceroute

- traces all calls when reaching an ip

```sh
traceroute google.com
```

### ping

- sends ICMP echo requests to IPs, you can block them in ufw
- you can use it to check downtime when you reboot a server. when it's off you get packet loss and get no replies

```sh
ping 8.8.8.8 # checks if target is online, endless
ping -c 4 'learnlinux.tv' # pings 4 times, it tells time spent, you have to get ~0% packet loss
```

### mtr

- combination of traceroute & ping

```sh
mtr google.com # keeps pinging, traces the requests, checks packet loss and times responses
mtr -i 2 google.com # pings for 2 sec
mtr -c 5 google.com # pings for 5 times
mtr -n google.com # doesn't resolve host names to ips
mtr [--json/--xml/--csv/--raw] -n google.com # doesn't resolve host names to ips
mtr -F some-log-file.log --csv -n google.com # logs in this file
```

### dig

- checks the DNS `A records` for ips for host names

```sh
dig google.com # querying the isp default dns server
dig @8.8.8.8 google.com # querying a specific dns server
```

```
;; ANSWER SECTION:
google.com.             55      IN      A       216.58.198.78
```

### nmap

- checks if host is up, **what ports are closed or open**, latency

```sh
nmap 127.0.0.1 another-host.com # scan 2 ips
nmap -v 127.0.0.1/24 # verbose, includes whole subnet
nmap -sV 127.0.0.1 # includes service info
nmap -A 127.0.0.1 # more info
nmap 127.0.0.1-4 --exclude 127.0.0.2-3 # will scan ips 127.0.0.1 & 127.0.0.4
nmap -sP 127.0.0.1/24 shows condensed info of the hosts online on this subnet
nmap -T5 2345.2435.324/24 # less accurace, much faster, T0 is much slower but much more accurate, use it to check for intrusion systems, T3 is default
```

### tcpdump

- sniffs traffic through all the osi layers

```sh
tcpdump -i enp0s3 -v host google.com # dumps traffic, verbose, for interface,
tcpdump -i enp0s3 -v src 192.168.1.96 and dst google.com and net 5984.5485.54/24 # source, destination, subnet
```

### netstat

- for tcp/udp network monitoring

```sh
netstat -ie # just like ip a, ifconfig
netstat -r # shows the routing table
netstat -c # continuous
```
