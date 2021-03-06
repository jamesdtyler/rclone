---
title: "pCloud"
description: "Rclone docs for pCloud"
date: "2017-10-01"
---

<i class="fa fa-cloud"></i> pCloud
-----------------------------------------

Paths are specified as `remote:path`

Paths may be as deep as required, eg `remote:directory/subdirectory`.

The initial setup for pCloud involves getting a token from pCloud which you
need to do in your browser.  `rclone config` walks you through it.

Here is an example of how to make a remote called `remote`.  First run:

     rclone config

This will guide you through an interactive setup process:

```
No remotes found - make a new one
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
name> remote
Type of storage to configure.
Choose a number from below, or type in your own value
 1 / Amazon Drive
   \ "amazon cloud drive"
 2 / Amazon S3 (also Dreamhost, Ceph, Minio)
   \ "s3"
 3 / Backblaze B2
   \ "b2"
 4 / Box
   \ "box"
 5 / Dropbox
   \ "dropbox"
 6 / Encrypt/Decrypt a remote
   \ "crypt"
 7 / FTP Connection
   \ "ftp"
 8 / Google Cloud Storage (this is not Google Drive)
   \ "google cloud storage"
 9 / Google Drive
   \ "drive"
10 / Hubic
   \ "hubic"
11 / Local Disk
   \ "local"
12 / Microsoft Azure Blob Storage
   \ "azureblob"
13 / Microsoft OneDrive
   \ "onedrive"
14 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ "swift"
15 / Pcloud
   \ "pcloud"
16 / QingCloud Object Storage
   \ "qingstor"
17 / SSH/SFTP Connection
   \ "sftp"
18 / Yandex Disk
   \ "yandex"
19 / http Connection
   \ "http"
Storage> pcloud
Pcloud App Client Id - leave blank normally.
client_id> 
Pcloud App Client Secret - leave blank normally.
client_secret> 
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes
n) No
y/n> y
If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth
Log in and authorize rclone for access
Waiting for code...
Got code
--------------------
[remote]
client_id = 
client_secret = 
token = {"access_token":"XXX","token_type":"bearer","expiry":"0001-01-01T00:00:00Z"}
--------------------
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y
```

See the [remote setup docs](/remote_setup/) for how to set it up on a
machine with no Internet browser available.

Note that rclone runs a webserver on your local machine to collect the
token as returned from pCloud. This only runs from the moment it opens
your browser to the moment you get back the verification code.  This
is on `http://127.0.0.1:53682/` and this it may require you to unblock
it temporarily if you are running a host firewall.

Once configured you can then use `rclone` like this,

List directories in top level of your pCloud

    rclone lsd remote:

List all the files in your pCloud

    rclone ls remote:

To copy a local directory to an pCloud directory called backup

    rclone copy /home/source remote:backup

### Modified time and hashes ###

pCloud allows modification times to be set on objects accurate to 1
second.  These will be used to detect whether objects need syncing or
not.  In order to set a Modification time pCloud requires the object
be re-uploaded.

pCloud supports MD5 and SHA1 type hashes, so you can use the
`--checksum` flag.

### Deleting files ###

Deleted files will be moved to the trash.  Your subscription level
will determine how long items stay in the trash.  `rclone cleanup` can
be used to empty the trash.
