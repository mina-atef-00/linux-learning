# LINUX DIRECTORIES

```py
❯ ls /

 bin -> usr/bin # normal binaries reside here (no sudo required)
 boot # boot files
 dev # device files, has ttys too, vda, vdb,sda,sdb hard drives
 etc # for config files, system and apps
 home # user files
 lib -> usr/lib
 lib32 -> usr/lib32
 lib64 -> usr/lib64
 libx32 -> usr/libx32
 lost+found
 media # when you mount external media it resides here, it's automatic mount
 mnt # temporary mount point for drives, it's manual
 opt # some 3rd party apps are here
 proc # has the running processes and system info
 root # the root user's home
 run
 sbin -> usr/sbin # sudoic bins here
 snap
 srv # service data, like http servers
 sys
 tmp # temporary files, will mostly get deleted at reboot. you can use it for storing stuff while your script finished running then delete the shit you made
 usr
 var # for web apps (/var/www) & log files & cron jobs & email inboxes
 swap.img
```

```py
❯ ls /usr

 bin # the real ones
 lib # has libs for the command binaries. they share libs
 local # command binaries that we create. like vscode commands are better to reside here
 sbin # the real ones
 share # has docs for most bins
```

- `/etc/network` >> networking settings
- `/etc/network/interfaces` >> your network interfaces config file
- `/proc/meminfo` >> has all the ram info
- `/proc/cpuinfo` >> has all the cpu info
