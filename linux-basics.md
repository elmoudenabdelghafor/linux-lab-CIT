# 🚀 Linux Lab — User Management, File Operations & Permissions

## 🎯 Objectives

By the end of this lab, students will be able to:

* Identify current user and system information.
* Create and manage users and groups.
* Perform essential file and directory operations.
* Configure file permissions (`rwx`), ownership, and access control.
* Apply all concepts through hands-on exercises.

Works on **Ubuntu (VMware)**.

---

# 👤 1. Who Am I? — System Identity

Before managing users, know who you are!

## 🔍 Basic identity commands

```bash
whoami              # show current username
id                  # show uid, gid, and groups
id username         # show info for specific user
groups              # list groups you belong to
groups username     # list groups for specific user
who                 # who is logged in
w                   # who is logged in + what they're doing
hostname            # show machine name
uname -a            # full system information
```

**Example output:**

```bash
$ whoami
omar

$ id
uid=1000(omar) gid=1000(omar) groups=1000(omar),27(sudo),999(docker)

$ groups
omar sudo docker
```

---

# 👥 2. Users & Groups Management

Linux is multi-user. Every user has a **UID** (User ID) and belongs to one or more **groups** (GID).

---

## ➕ Creating users — `adduser` / `useradd`

```bash
sudo adduser student1           # interactive (recommended)
sudo useradd -m student2        # manual (-m creates home dir)
```

**Set/change password:**

```bash
sudo passwd student1
```

**Example:**

```bash
sudo adduser alice
# Enter password and user details
```

---

## 👁️ View user information

```bash
cat /etc/passwd                 # all users
grep alice /etc/passwd          # specific user
id alice
finger alice                    # detailed info (if installed)
```

**Example `/etc/passwd` entry:**

```
alice:x:1001:1001:Alice Smith:/home/alice:/bin/bash
```

Breakdown: `username:password:UID:GID:comment:home:shell`

---

## ✏️ Modifying users — `usermod`

```bash
sudo usermod -l newname oldname     # rename user
sudo usermod -d /new/home alice     # change home directory
sudo usermod -s /bin/zsh alice      # change shell
sudo usermod -L alice               # lock account
sudo usermod -U alice               # unlock account
```

---

## 🗑️ Deleting users — `userdel`

```bash
sudo userdel student1               # delete user (keep home)
sudo userdel -r student1            # delete user + home directory
```

⚠️ **Warning:** `-r` removes all user files permanently!

---

## 👥 Creating groups — `groupadd`

```bash
sudo groupadd developers
sudo groupadd -g 5000 admins        # specify GID
```

---

## 🔗 Adding users to groups — `usermod`

```bash
sudo usermod -aG developers alice   # add to group (keep existing)
sudo usermod -g developers alice    # change primary group
```

**Key difference:**

* `-aG` = **append** to supplementary groups
* `-g` = change **primary** group

**Example — Add multiple users to a group:**

```bash
sudo usermod -aG developers alice
sudo usermod -aG developers bob
sudo usermod -aG developers charlie
```

---

## 📋 View group information

```bash
cat /etc/group                      # all groups
grep developers /etc/group          # specific group
getent group developers             # query group database
```

**Example `/etc/group` entry:**

```
developers:x:1002:alice,bob,charlie
```

Breakdown: `groupname:password:GID:members`

---

## ✏️ Modifying groups — `groupmod`

```bash
sudo groupmod -n newname oldname    # rename group
sudo groupmod -g 6000 developers    # change GID
```

---

## 🗑️ Deleting groups — `groupdel`

```bash
sudo groupdel developers
```

Cannot delete a user's primary group while user exists.

---

## 🔄 Switch user — `su`

```bash
su alice                # switch user (keeps environment)
su - alice              # switch user (loads alice's environment)
exit                    # return to previous user
```

**Difference:**

* `su alice` → stays in current directory
* `su - alice` → goes to alice's home, loads her shell config

---

## 🔒 Sudo privileges

```bash
sudo command                        # run as superuser
sudo -u alice command               # run as alice
sudo -l                             # list allowed commands
sudo -i                             # interactive root shell
```

**Grant sudo access:**

```bash
sudo usermod -aG sudo alice         # Ubuntu/Debian
sudo usermod -aG wheel alice        # CentOS/RHEL
```

**Or edit sudoers file:**

```bash
sudo visudo
```

Add line: `alice ALL=(ALL:ALL) ALL`

---

# 📂 3. File & Directory Management

Now that we can manage users, let's manage their files!

---

## 🔍 Listing files — `ls`

```bash
ls                  # list files
ls -l               # detailed list (permissions, size, owner)
ls -lh              # human-readable sizes (KB, MB, GB)
ls -a               # show hidden files (starting with .)
ls -la              # detailed + hidden
ls -lt              # sort by modification time
ls -lS              # sort by size
ls -R               # recursive listing
```

**Hidden files example:**

```bash
ls -a ~
# Shows: .bashrc .profile .ssh/ .config/
```

---

## 📄 Creating files — `touch`

```bash
touch file.txt                  # create empty file
touch file1.txt file2.txt       # create multiple
touch -t 202301151200 old.txt   # set specific timestamp
```

**File editors:**

```bash
nano file.txt       # beginner-friendly
vim file.txt        # powerful but steep learning curve
nvim file.txt       # modern vim
gedit file.txt      # GUI editor (if available)
```

---

## 📝 Viewing file content

```bash
cat file.txt                    # show entire file
cat file1.txt file2.txt         # concatenate files
head file.txt                   # first 10 lines
head -n 5 file.txt              # first 5 lines
tail file.txt                   # last 10 lines
tail -n 20 file.txt             # last 20 lines
tail -f /var/log/syslog         # follow file updates (logs)
less file.txt                   # paginated view (q to quit)
more file.txt                   # simple paginated view
```

---

## ✍️ Writing to files

```bash
echo "Hello World" > file.txt       # overwrite file
echo "New line" >> file.txt         # append to file
cat > file.txt                      # type until Ctrl+D
cat << EOF > file.txt               # heredoc (multi-line)
Line 1
Line 2
EOF
```

**Example:**

```bash
echo "My first Linux file" > notes.txt
echo "Learning is fun!" >> notes.txt
cat notes.txt
```

---

## 📁 Creating directories — `mkdir`

```bash
mkdir test                      # create directory
mkdir dir1 dir2 dir3            # create multiple
mkdir -p project/src/main       # create nested (parents too)
mkdir -m 755 public             # create with permissions
```

**The `-p` option** creates parent directories if they don't exist.

**Example:**

```bash
mkdir -p ~/workspace/python/projects
```

---

## 🧭 Navigation — `cd`, `pwd`

```bash
pwd                 # print working directory
cd /etc             # go to /etc
cd ~                # go to home directory
cd                  # go to home (same as cd ~)
cd ..               # go up one level
cd ../..            # go up two levels
cd -                # go to previous directory
cd /                # go to root
```

**Example navigation:**

```bash
$ pwd
/home/omar
$ cd /var/log
$ pwd
/var/log
$ cd -              # back to /home/omar
```

---

## 📑 Copy files & directories — `cp`

```bash
cp source.txt dest.txt              # copy file
cp file.txt /tmp/                   # copy to directory
cp file1.txt file2.txt /backup/     # copy multiple
cp -r folder1 folder2               # copy directory (recursive)
cp -i source.txt dest.txt           # interactive (confirm overwrite)
cp -u source.txt dest.txt           # update (only if newer)
cp -a folder backup                 # archive mode (preserve all)
```

**Example:**

```bash
cp project/ project_backup/
# Error! Need -r for directories

cp -r project/ project_backup/      # Correct
```

---

## 🚚 Move & rename — `mv`

```bash
mv oldname.txt newname.txt          # rename file
mv file.txt /tmp/                   # move file
mv file1.txt file2.txt /backup/     # move multiple
mv -i source.txt dest.txt           # interactive (ask before overwrite)
mv -u source.txt dest.txt           # update only if newer
mv dir1 dir2                        # rename/move directory
```

**Example:**

```bash
mv notes/ old_notes/                # rename directory
mv *.txt Documents/                 # move all txt files
```

---

## 🗑️ Delete — `rm`, `rmdir`

```bash
rm file.txt                         # delete file
rm file1.txt file2.txt              # delete multiple
rm -i file.txt                      # interactive (confirm)
rm -r folder/                       # delete directory + contents
rm -rf folder/                      # force delete (DANGEROUS!)
rmdir empty_folder/                 # delete empty directory only
```

⚠️ **WARNING:** `rm -rf /` destroys the entire system — **NEVER run it!**

**Safe practice:**

```bash
rm -i important.txt     # Always use -i for important files
```

---

## 📦 File information — `stat`, `file`

```bash
stat file.txt           # detailed metadata (size, timestamps, perms)
file file.txt           # determine file type
file *                  # check type of all files
```

**Example output:**

```bash
$ file script.sh
script.sh: Bash script, ASCII text executable

$ file image.jpg
image.jpg: JPEG image data, JFIF standard
```

---

## 🔗 Links — Hard & Symbolic

**Hard link** = another name for the same file (same inode)

```bash
ln original.txt hardlink.txt
```

**Symbolic link (symlink)** = shortcut pointing to original

```bash
ln -s /path/to/original.txt symlink.txt
ln -s /etc/nginx/sites-available/mysite /etc/nginx/sites-enabled/
```

**Key differences:**

| Feature       | Hard Link                | Symlink                    |
| ------------- | ------------------------ | -------------------------- |
| Different file? | No (same inode)          | Yes (pointer)              |
| Works across filesystems? | No                       | Yes                        |
| Works with directories? | No                       | Yes                        |
| Breaks if original deleted? | No (still works)         | Yes (broken link)          |

**Example:**

```bash
echo "data" > original.txt
ln original.txt hard.txt        # hard link
ln -s original.txt soft.txt     # symlink

rm original.txt
cat hard.txt        # Still works!
cat soft.txt        # Error: No such file
```

---

## 🔎 Search files — `find`

```bash
find . -name "*.txt"                # find by name
find /home -user alice              # files owned by alice
find . -type f                      # files only
find . -type d                      # directories only
find /var -size +10M                # larger than 10MB
find . -mtime -7                    # modified in last 7 days
find . -name "*.log" -delete        # find and delete
find . -perm 644                    # specific permissions
```

**Example — Find large files:**

```bash
find ~ -type f -size +100M -exec ls -lh {} \;
```

---

## 📊 Disk usage — `du`, `df`

```bash
du -sh directory/           # size of directory
du -sh *                    # size of all items
du -h --max-depth=1         # top-level sizes only
df -h                       # disk space (all filesystems)
df -h /home                 # specific filesystem
```

**Example:**

```bash
$ du -sh Downloads/
2.3G    Downloads/

$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   35G   13G  73% /
```

---

# 🔒 4. File Permissions & Ownership

Every file/directory has:

* **Owner** (user)
* **Group**
* **Others** (everyone else)

Each can have: **read (r)**, **write (w)**, **execute (x)**

---

## 📖 Understanding permissions

**Example output:**

```bash
$ ls -l script.sh
-rwxr-xr-- 1 omar developers 2048 Jan 28 script.sh
```

**Breakdown:**

```
- rwx r-x r--
│ │   │   │
│ │   │   └─ Others: read only
│ │   └───── Group: read + execute
│ └───────── Owner: read + write + execute
└─────────── File type (- = file, d = directory, l = link)
```

**Permission meanings:**

| Permission | Files | Directories |
| ---------- | ----- | ----------- |
| **r** (read) | View content | List contents (`ls`) |
| **w** (write) | Modify file | Create/delete files inside |
| **x** (execute) | Run as program | Enter directory (`cd`) |

---

## ✏️ Changing permissions — `chmod`

### Symbolic mode:

```bash
chmod u+x file.txt              # add execute for owner
chmod g-w file.txt              # remove write from group
chmod o=r file.txt              # set others to read-only
chmod a+r file.txt              # add read for all (a = all)
chmod u+rwx,g+rx,o-rwx file.txt # multiple changes
```

**Who codes:**

* `u` = user (owner)
* `g` = group
* `o` = others
* `a` = all

**Operators:**

* `+` = add
* `-` = remove
* `=` = set exactly

---

### Numeric mode:

Each permission has a value:

| Permission | Value |
| ---------- | ----- |
| `r` (read) | 4     |
| `w` (write) | 2     |
| `x` (execute) | 1     |

Add them up for each category:

```bash
chmod 755 script.sh     # rwx r-x r-x
chmod 644 document.txt  # rw- r-- r--
chmod 700 private/      # rwx --- ---
chmod 600 secret.key    # rw- --- ---
chmod 777 shared/       # rwx rwx rwx (NOT RECOMMENDED!)
```

**Common patterns:**

* `755` — executable files, public directories
* `644` — regular files (documents, configs)
* `700` — private directories
* `600` — private files (SSH keys, passwords)

**Calculation example:**

* `rwx` = 4+2+1 = **7**
* `r-x` = 4+0+1 = **5**
* `r--` = 4+0+0 = **4**

So `rwxr-xr--` = **754**

---

## 👑 Changing ownership — `chown`, `chgrp`

```bash
sudo chown alice file.txt               # change owner
sudo chown alice:developers file.txt    # change owner + group
sudo chown :developers file.txt         # change group only
sudo chown -R alice:alice /home/alice   # recursive
sudo chgrp developers project/          # change group only
```

**Example — Shared project:**

```bash
sudo mkdir /shared
sudo chown root:developers /shared
sudo chmod 770 /shared
```

Now:

* Root can do anything
* Developers group has full access
* Others have no access

---

## 🔐 Special permissions

### Setuid (4) — Run as file owner

```bash
chmod u+s /usr/bin/program
chmod 4755 program
```

Example: `passwd` command runs as root even when you're not root.

---

### Setgid (2) — Inherit group ownership

```bash
chmod g+s /shared/project
chmod 2775 /shared/project
```

Files created inside inherit the directory's group.

---

### Sticky bit (1) — Delete only if owner

```bash
chmod +t /tmp
chmod 1777 /tmp
```

Users can only delete their own files (even with write permission).

**Example — `/tmp` permissions:**

```bash
$ ls -ld /tmp
drwxrwxrwt 15 root root 4096 Jan 28 /tmp
             ^
             └─ sticky bit (t)
```

---

## 🔍 View permissions

```bash
ls -l file.txt              # long format
stat file.txt               # detailed metadata
getfacl file.txt            # ACLs (if used)
```

---

# 🧪 5. Hands-On Lab Exercises

## 📚 Lab 1 — User & Group Management

**Scenario:** Set up a development team.

1. Check your identity:

   ```bash
   whoami
   id
   groups
   ```

2. Create users:

   ```bash
   sudo adduser dev1
   sudo adduser dev2
   sudo adduser manager
   ```

3. Create groups:

   ```bash
   sudo groupadd developers
   sudo groupadd management
   ```

4. Add users to groups:

   ```bash
   sudo usermod -aG developers dev1
   sudo usermod -aG developers dev2
   sudo usermod -aG management manager
   ```

5. Verify:

   ```bash
   groups dev1
   groups manager
   id dev1
   ```

6. Switch user and test:

   ```bash
   su - dev1
   whoami
   id
   exit
   ```

---

## 📚 Lab 2 — File Operations

1. Create directory structure:

   ```bash
   mkdir -p ~/linux_lab/docs/archive
   ```

2. Create files:

   ```bash
   cd ~/linux_lab
   touch file1.txt file2.txt file3.txt
   echo "Learning Linux is awesome!" > file1.txt
   ```

3. Copy operations:

   ```bash
   cp file1.txt file1_backup.txt
   cp -r docs docs_backup
   ```

4. Move operations:

   ```bash
   mv file3.txt docs/
   mv file2.txt renamed.txt
   ```

5. Create links:

   ```bash
   ln file1.txt hardlink.txt
   ln -s file1.txt symlink.txt
   ls -li      # compare inode numbers
   ```

6. View file info:

   ```bash
   stat file1.txt
   file file1.txt
   ```

7. Search:

   ```bash
   find . -name "*.txt"
   find . -type d
   ```

8. Size check:

   ```bash
   du -sh docs/
   du -sh *
   ```

9. Create large file:

   ```bash
   dd if=/dev/zero of=bigfile.dat bs=1M count=10
   ls -lh bigfile.dat
   ```

10. Clean up:

    ```bash
    rm bigfile.dat
    rm -r docs_backup/
    ```

---

## 📚 Lab 3 — Permissions & Ownership

**Scenario:** Create a shared project with proper access control.

1. Create project structure:

   ```bash
   sudo mkdir /projects
   sudo mkdir /projects/web_app
   ```

2. Create group and add users:

   ```bash
   sudo groupadd webdevs
   sudo usermod -aG webdevs dev1
   sudo usermod -aG webdevs dev2
   ```

3. Set ownership:

   ```bash
   sudo chown root:webdevs /projects/web_app
   ```

4. Set permissions (group collaboration):

   ```bash
   sudo chmod 2770 /projects/web_app
   ```

   Breakdown:
   * `2` = setgid (files inherit group)
   * `770` = owner + group full access, no others

5. Test as dev1:

   ```bash
   su - dev1
   cd /projects/web_app
   touch index.html
   echo "<h1>Hello World</h1>" > index.html
   ls -l
   ```

   Notice the file's group is `webdevs` (because of setgid).

6. Test as dev2:

   ```bash
   su - dev2
   cd /projects/web_app
   echo "<p>Added by dev2</p>" >> index.html
   ```

   It works! Both devs can collaborate.

7. Create private file:

   ```bash
   touch secret.txt
   chmod 600 secret.txt
   ls -l secret.txt
   ```

8. Create public read-only:

   ```bash
   sudo mkdir /projects/public
   sudo chmod 755 /projects/public
   echo "Public info" | sudo tee /projects/public/readme.txt
   sudo chmod 644 /projects/public/readme.txt
   ```

---

## 📚 Lab 4 — Advanced Permissions

**Scenario:** Create a shared directory where users can only delete their own files.

1. Create directory:

   ```bash
   sudo mkdir /shared_space
   sudo chmod 1777 /shared_space
   ```

   `1777` = sticky bit + full access for all

2. Test as different users:

   ```bash
   # As dev1
   su - dev1
   touch /shared_space/dev1_file.txt
   exit

   # As dev2
   su - dev2
   touch /shared_space/dev2_file.txt
   rm /shared_space/dev1_file.txt     # Permission denied!
   exit
   ```

3. Verify sticky bit:

   ```bash
   ls -ld /shared_space
   # drwxrwxrwt ... /shared_space
   #         ^-- sticky bit
   ```

---

## 📚 Lab 5 — Permission Troubleshooting

Given file: `mystery.txt`

1. Create the file:

   ```bash
   touch mystery.txt
   chmod 000 mystery.txt
   ```

2. Try to read it:

   ```bash
   cat mystery.txt     # Permission denied
   ```

3. Fix permissions:

   ```bash
   chmod u+r mystery.txt
   cat mystery.txt     # Now works!
   ```

4. Set specific permissions:
   * Owner: read + write
   * Group: read only
   * Others: no access

   ```bash
   chmod u=rw,g=r,o= mystery.txt
   # or
   chmod 640 mystery.txt
   ```

5. Verify:

   ```bash
   ls -l mystery.txt
   # -rw-r----- ...
   ```

---

# 📝 6. Command Cheat Sheet

## Identity

```bash
whoami, id, groups, who, w, hostname
```

## Users

```bash
sudo adduser, sudo useradd, sudo userdel, sudo usermod, sudo passwd
```

## Groups

```bash
sudo groupadd, sudo groupdel, sudo groupmod, getent group
```

## Files

```bash
ls, cd, pwd, mkdir, touch, cat, echo, cp, mv, rm, ln, stat, file
```

## Search & Size

```bash
find, du, df
```

## Permissions

```bash
chmod, chown, chgrp, umask
```

## File Content

```bash
cat, head, tail, less, more, nano, vim
```

---

# 🎯 7. Permission Quick Reference

| Numeric | Symbolic | Meaning |
| ------- | -------- | ------- |
| 777 | rwxrwxrwx | Everyone can do everything (AVOID!) |
| 755 | rwxr-xr-x | Owner full, others read+execute |
| 644 | rw-r--r-- | Owner write, others read |
| 700 | rwx------ | Owner only, private |
| 600 | rw------- | Owner read/write only, very private |
| 750 | rwxr-x--- | Owner full, group read+execute |
| 640 | rw-r----- | Owner write, group read |

---

# 🎉 End of Lab

Congratulations! You now understand:

✅ User identity commands  
✅ Creating and managing users and groups  
✅ File and directory operations  
✅ Linux permission system  
✅ Ownership and access control  

These skills are **essential** for **Cloud Engineers, DevOps Engineers, SysAdmins, and Security Engineers**.

Keep practicing! 🚀
