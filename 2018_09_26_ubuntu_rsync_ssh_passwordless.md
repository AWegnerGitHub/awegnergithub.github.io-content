Title: Backing up Ubuntu laptop to Ubuntu Server with passwordless rsync
Date: 2018-09-26 12:30
Tags: technical
Category: Technical Solutions
Slug: ubuntu-backup-rsync
Summary: The server has been running and the laptop needs to be backed up. This walks through how I did it.
Status: published

[TOC]

## Introduction

The server has been running for almost nine months. It's been backing up family data and pictures from phones without any problems. Now it's time to back up the laptop because I have the space and really should make sure the stuff that isn't work related (ie. the stuff that is in the work git repositories) is also backed up. 

Enter [`rsync`][1]. 

## How To

My goal is to automatically back up my home directory from the laptop to the server on a daily basis. This will provide a once a day backup and if I need to do more than that in the future, it will be as easy as modifying the final cronjob that I'll use.

### SSH Key

The first step is setting up an SSH key so that I don't have to manually provide a password. I can, in the future, add restrictions on the server side as to what this particular key will be able to do too. I'm not doing that today though, because I don't open SSH to the outside world. 

The first thing to do is generate a new key. I already have an SSH key configured, but it has a password. On the laptop, run the follwoing:

    ssh-keygen -t rsa -b 2048 -f ~/.ssh/laptop-rsync-key
	
When asked to enter a passphrase, simply press enter and then enter again to confirm the empty passphrase.
	
This will put `laptop-rsync-key` and `laptop-rsync-key.pub` in my user's `.ssh/` directory.

### Copy the public key to the server 

Next, we need to copy the public key that was just generated to the server. 

    scp ~/.ssh/laptop-rsync-key.pub andy@192.168.140.187:~/.ssh

Once it's been copied, log into the server. Now you need to add this key to the `authorized_keys`.

	cd ~/.ssh
    cat laptop-rsync-key.pub >> authorized_keys
	
### rsync command

The final command to back up my home directory is pretty simple. This command is going to tell `rsync` to use the new SSH key that was just created, to exclude all dot files and directories, and to delete anything that has been removed on the laptop from the server. The backup will go in `~/backup/laptop` on the server.

    rsync -a -e "ssh -i ~/.ssh/laptop-rsync-key" ~/ andy@nas:~/backup/laptop --exclude=".*" --exclude=".*/" --delete
	
Once I confirmed this worked, I added it to my user's crontab on the laptop. It will run once a day now.
	
## Next steps

The next steps I'll take be taking are to restrict the new SSH key on the server to only allow it to perform `rsync` tasks. This can be done by slightly modifying the appropriate line in `authorized_keys`. I'll see how this daily, single, back up works for a while. If I need to, I may change it to a rotating weekly backup. I don't forsee that right now, but I need a few weeks of seeing how this works and if the single day is good enough. 
	
 [1]: https://rsync.samba.org/