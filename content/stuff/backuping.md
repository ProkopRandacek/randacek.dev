---
permalink: /stuff/backuping
title: Backuping
layout: default
---
# Backuping

They always say that its important but I never do it. Well when I broke my Linux installation for the 3rd time, I decided to finally do it.  

I didn't want to use any existing program because I had a specific idea how I want my data to be backed up and I was too lazy to search for a existing solution that would satisfy me. So I made my own. It ended up being so simple and minimalistic, that I though that maybe somebody else would appreciate it too :D  

### My idea was:  
1. Command line (duh)
2. My own server
3. Choose specific directories or files
4. Backup is a single zip file
5. Installed packages are backed up too
6. Backup on command and boot

#### Lets start with choosing files to be backed up
add_to_backup_list.fish
```fish
function add_to_backup_list
	echo $PWD/$argv >> ~/backuplist
end
```
I think that the hardest part was coming up with a name for the function and I definitely fucked up that part.  

### Backuping installed packages
packup.sh
```bash
#!/usr/bin/bash
echo writting package lists &&
pacman -Qqen > ~/pkglist.txt &&
pacman -Qqem > ~/pkglist_aur.txt &&
cat ~/pkglist.txt ~/pkglist_aur.txt > ~/pkglist_full.txt &&
echo zipping package lists &&
zip -r ~/pkglists.zip ~/pkglist.txt ~/pkglist_aur.txt ~/pkglist_full.txt 1> /dev/null &&
echo uploading package lists &&
rsync ~/pkglists.zip prokop@randacek.dev:/home/prokop/backups/$(cat /etc/hostname)/
rm ~/pkglist.txt ~/pkglist_aur.txt ~/pkglist_full.txt ~/pkglists.zip
```
Pretty straight forward. `pacman -Qqe{n, m}` outputs installed packages (n form official repos, m from AUR) to text files that are then zipped and send to my server.  
Interesting is using `&&` to connect commands. It makes the whole script a logical expression. When command ends, the exit value is used as value for the expression. Weirdly, 0 is true and everything else is false. As soon as any command ends with non-zero exit value, the whole expression is false and bash doesn't need to execute rest of the script. Therefore, when any command fails, it wont continue executing the script.  
Bash is weird.  

### Backuping specified files
backup.sh
```bash
#!/usr/bin/bash
~/scripts/backup/packup.sh &&
echo zipping backuplist &&
date > ~/backuptime &&
zip ~/backup.zip $(cat ~/backuplist) ~/backuptime ~/backuplist -r 1> /dev/null &&
du ~/backup.zip -h &&
echo uploading backup &&
rsync ~/backup.zip prokop@randacek.dev:/home/prokop/backups/$(cat /etc/hostname)/ --progress &&
rm ~/backup.zip &&
```
I was kinda expecting that I would have to replace newlines with spaces when using backuplist as input for zip so the fact that I don't have to was really pleasant surprise. The reason why backuping scripts are not fish functions is because I found out that running fish function from systemd service is not trivial and it was easier to just write it in bash.  
Note: [setup ssh key](https://docs.github.com/en/enterprise/2.15/user/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) and use `ssh-copy-id` to avoid typing password every time.  

### Run it at boot
```
[Unit]
Description=Backup script

[Service]
Type=simple
ExecStart=sudo -u prokop /home/prokop/scripts/backup/backup.sh

[Install]
WantedBy=multi-user.target
```
`sudo -u prokop` because services are by default executed by root and root has wrong home directory. It came to my mind that anyone could replace the `backup.sh` file and run anything on my machine on boot. Luckily most of my people that would potentially have access to my machines have no idea how to use Linux.  
