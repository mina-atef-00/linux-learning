# SOME LINUX COMMANDS

## Basics

```sh
whoami # print user
man # ual
`clear`
[] # optional
pwd # print working directory
`ls --all --human-readable --size -S`
cd # is posix
`mkdir -p /all/new/stuff/not/there`
`touch woman apple.txt misterman.md bruh.py some-other-file.js`
`rm --verbose`
rmdir # for empty dirs == rm -d
`rm --recursive --force --interactive`
xdg-open # is just like macs open
mv -t /destination/dir # moves file from destination to current directory
mv --update # move only when src file is newer than dest file
cp -r --perserve`
cp -s # makes symbolic links
```

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
head -l 5 # first 5
tail -n 5 # last 5 lines

tail -F # keeps monitoring the file for changes _perfect for logs_

date # shows date
```

---

## Piping

```sh
> # writes to file. doesn't append. automatically creates
>> # appends to file
| # pipes the output to another command
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
sort --ignore-leading-blanks --dictionary-order --month-sort --numeric-sort --reverse`
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

```sh
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
du -h | sort -nr | head -l 5 # sort by size, get top 5
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

## fmt

```sh
fmt -w 20 file.txt # outputs file to be 20 char long, to fit small terminals
fmt -u file.txt # adds a space after each word, 2 spaces after each sentence
fmt -t # indents first line of paragraph
fmt -s # splits longer lines
```
