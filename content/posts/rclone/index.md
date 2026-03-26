+++
title = "Rclone - Sync your files to cloud storage"
date = "2024-10-08"
description = "Learn the unified tool to manage files over 70+ cloud storage providers"
[taxonomies]
tags = ["sync-tool"]
+++

rclone is a command line tool to manage cloud storage providers such as S3, Google Drive, OneDrive, Dropbox, NextCloud and many more (Feeling happy with 70 other providers :ps facebook reaction mantra). It is often termed as **"The Swiss army knife of cloud storage"**. Learn one program to rule all of the cloud providers :)

## Installation

Follow the [installation guide](https://rclone.org/install/) according to your operating system to install rclone.

## Syntax

Its syntax is like this:

`rclone subcommand [options] <parameters...>`

A `subcommand` is a rclone operation such as `copy`, `sync`, `ls`

### Useful Flags

- `-i`, `--interactive` - interactive, asks for your input before doing anything permanent, similar to dry run but asks you
- `-v`, `--verbose` - verbose logging, useful for watching the progress
- `-n`, `--dry-run` Do a trial without any permanent change

## Configuring

```bash
rclone config
```

- Press `n` for **new remote**
- Give this remote a memorable and something simple like **personal-onedrive** or **personal-gDrive**. This remote name should be a unique one.
- Enter the corresponding number of the cloud storage that you want to configure in the next step. 
  - Type 18 for google drive in this case. This sequence and numbering gets changed with new rclone updates since new cloud providers are supported in newer rclone releases. 

Be careful, **google cloud storage is not the same as google drive.**

```bash
Option Storage.
Type of storage to configure.
Choose a number from below, or type in your own value.
 1 / 1Fichier
   \ (fichier)
 2 / Akamai NetStorage
   \ (netstorage)
 3 / Alias for an existing remote
   \ (alias)
 4 / Amazon S3 Compliant Storage Providers including AWS, Alibaba, ArvanCloud, Ceph, ChinaMobile, Cloudflare and others
   \ (s3)
 5 / Backblaze B2
   \ (b2)
 6 / Better checksums for other remotes
   \ (hasher)
 7 / Box
   \ (box)
...
...
...
15 / FTP
   \ (ftp)
16 / Files.com
   \ (filescom)
17 / Gofile
   \ (gofile)
18 / Google Cloud Storage (this is not Google Drive)
   \ (google cloud storage)
19 / Google Drive
   \ (drive)
20 / Google Photos
   \ (google photos)
...
...
...
```

- Enter your client id, client secret or leave it as empty if you don't know about it. client id and client secret are asked seprately.
- Define the scope for this rclone remote like whether you want the rclone to have read only permissions or read write or full permissions over the whole drive. Most of the times, I give full access to the files for ease of use :)
- Keep pressing enter for all questions that you don't know of or otherwise a browser windows pop up asking for login to your google account to give permissions to rclone. Authorize the rclone to access your files in cloud storage (in this case google drive).

## Listing Files / Directories

rclone offers similar syntax for listing files and directories as unix based system does using `ls` command.

> The `:` after the remote name is important.

```bash
# listing all the files and directories recursively

# this would list down the files and directories inside of the Personal folder
rclone ls <remote-name>:Personal  
rclone ls <remote-name>:Development/blogs

# If you want to list down only the directories and not files,
# rclone have got you covered
rclone lsd <remote-name>:<sub-directory>

# only list down the directories inside of the Personal
rclone lsd <remote-name>:Personal 
# list down the directories inside of Development/blogs
rclone lsd <remote-name>:Development/blogs 

# list down only the files
rclone lsf <remote-name>:
# list down only the files inside of Personal directory
rclone lsf <remote-name>:Personal
```

## Syncing / Copy

After having rclone configured with the provider of your choice, you can try copying and syncing things to and from the remote. rclone has both the one way and also [bidirectional syncing](https://rclone.org/bisync/) capabilities.

Here in this snippet, uni-directional sync is being demonstrated. This effectively means you can either download or upload content to and from the remote.

```bash
# remote host means configured rclone remote 
# source - can be a remote or local file system
# destination - can either be a remote or local file system
rclone <any flags> sync <source> <destination>

# if the <source> is a remote host and <destination> is local system,
# this effectively means to download the remote content locally
rclone -v sync <remote-name>: <local file system>
# replacing <remote-name> with personal-gdrive and
# <local file system> with a local path
rclone -v sync personal-gdrive: ~/Downloads/rclone/personal-drive

# now if the <source> is a local file system path and <destination> is remote,
# then this will be similar to file upload scenario
# following command will sync the local folder to remote location
rclone -v sync ~/Downloads/rclone/personal-drive personal-gdrive:
```

## Mounting as Drive

I use rclone mainly for google drive. I was used to syncing google drive files using [Google Drive Desktop client](https://support.google.com/a/users/answer/13022292?hl=en#drive_desktop_install), but missed this on Linux as it was available only for the Windows and MacOS. It would mount my google drive
