# 🚀 Linux Basics Lab — Users, Files & Permissions

## 🎯 What You'll Learn

* Create and manage users
* Work with files and folders
* Understand and set permissions

**Works on Ubuntu (VMware)**

---

# 👤 1. Who Am I?

Before doing anything, let's find out who you are!

## Basic Commands

```bash
whoami              # your username
id                  # your user ID and groups
groups              # groups you belong to
hostname            # computer name
```

**Try it:**

```bash
$ whoami
omar

$ groups
omar sudo
```

---

# 👥 2. Managing Users

Every user in Linux has a username and belongs to groups.

## Creating a User

```bash
sudo adduser student1
```

This will ask you to:
* Set a password
* Enter user details (you can skip these)

## Viewing Users

```bash
id student1                    # see user info
grep student1 /etc/passwd      # see user details
```

## Deleting a User

```bash
sudo userdel student1          # delete user only
sudo userdel -r student1       # delete user + their files
```

⚠️ **Be careful with `-r` — it deletes everything!**

## Switching Users

```bash
su - student1       # become student1
exit                # go back to your account
```

---

# 👥 3. Managing Groups

Groups let multiple users share access to files.

## Creating Groups

```bash
sudo groupadd developers
```

## Adding Users to Groups

```bash
sudo usermod -aG developers student1
```

* `-aG` means "add to group" (keeps other groups too)

## Viewing Groups

```bash
groups student1                # see what groups student1 is in
grep developers /etc/group     # see who's in developers group
```

## Deleting Groups

```bash
sudo groupdel developers
```

---

# 📂 4. Working with Files

## Listing Files

```bash
ls              # list files
ls -l           # detailed list (shows permissions)
ls -a           # show hidden files (start with .)
ls -lh          # sizes in KB/MB/GB
```

## Creating Files

```bash
touch notes.txt                    # create empty file
echo "Hello Linux" > file.txt      # create with content
echo "More text" >> file.txt       # add to existing file
```

## Viewing Files

```bash
cat file.txt        # show entire file
head file.txt       # first 10 lines
tail file.txt       # last 10 lines
```

## Creating Folders

```bash
mkdir projects                    # create folder
mkdir -p work/code/python         # create nested folders
```

The `-p` option creates parent folders automatically.

## Copying

```bash
cp file.txt backup.txt            # copy file
cp -r folder1 folder2             # copy folder
```

**Remember:** Use `-r` for folders!

## Moving & Renaming

```bash
mv oldname.txt newname.txt        # rename
mv file.txt Documents/            # move file
```

## Deleting

```bash
rm file.txt                       # delete file
rm -r folder/                     # delete folder
rm -i important.txt               # ask before deleting
```

⚠️ **Warning:** `rm -rf` deletes everything without asking — be careful!

## Navigation

```bash
pwd             # where am I?
cd Documents    # go to Documents
cd ..           # go up one level
cd ~            # go to home folder
cd -            # go to previous location
```

---

# 🔒 5. Understanding Permissions

Every file has permissions for three groups:
* **Owner** (the user who owns it)
* **Group** (the group that owns it)
* **Others** (everyone else)

Each can have three permissions:
* **r** = read (view the file)
* **w** = write (change the file)
* **x** = execute (run the file as a program)

## Reading Permissions

```bash
$ ls -l script.sh
-rwxr-xr-- 1 omar developers 2048 Jan 28 script.sh
```

**What does this mean?**

```
- rwx r-x r--
│ │   │   │
│ │   │   └─ Others: read only
│ │   └───── Group: read + execute
│ └───────── Owner: read + write + execute
└─────────── Type: - = file, d = folder
```

## Changing Permissions — Easy Way

```bash
chmod u+x file.txt      # give owner execute permission
chmod g-w file.txt      # remove group write permission
chmod o+r file.txt      # give others read permission
chmod a+r file.txt      # give everyone read permission
```

**Who codes:**
* `u` = user (owner)
* `g` = group
* `o` = others
* `a` = all (everyone)

**What to do:**
* `+` = add
* `-` = remove
* `=` = set exactly

## Changing Permissions — Number Way

Each permission has a number:
* `r` (read) = 4
* `w` (write) = 2
* `x` (execute) = 1

Add them up:

```bash
chmod 755 script.sh     # rwx r-x r-x (most programs)
chmod 644 doc.txt       # rw- r-- r-- (most documents)
chmod 700 private/      # rwx --- --- (private folder)
chmod 600 secret.txt    # rw- --- --- (private file)
```

**Common patterns:**
* `755` — programs and shared folders
* `644` — regular documents
* `700` — your private folders
* `600` — your private files

## Changing Ownership

```bash
sudo chown alice file.txt                  # change owner
sudo chown alice:developers file.txt       # change owner + group
sudo chown -R alice projects/              # change folder + everything inside
```

---

# 🧪 6. Practice Exercises

## Exercise 1: Create Users

```bash
# 1. Create two users
sudo adduser dev1
sudo adduser dev2

# 2. Create a group
sudo groupadd coders

# 3. Add users to group
sudo usermod -aG coders dev1
sudo usermod -aG coders dev2

# 4. Check it worked
groups dev1
groups dev2
```

## Exercise 2: File Practice

```bash
# 1. Create a project folder
mkdir -p ~/my_project/code

# 2. Go there
cd ~/my_project

# 3. Create some files
touch README.md app.py notes.txt

# 4. Add content
echo "# My First Project" > README.md
echo "print('Hello World')" > app.py

# 5. View them
cat README.md
cat app.py

# 6. Copy app.py
cp app.py app_backup.py

# 7. List everything
ls -l

# 8. Check folder size
du -sh ~/my_project
```

## Exercise 3: Permissions Practice

```bash
# 1. Create a test file
touch test.txt
echo "Secret data" > test.txt

# 2. Check current permissions
ls -l test.txt

# 3. Make it private (only you can read/write)
chmod 600 test.txt
ls -l test.txt

# 4. Make it readable by everyone
chmod 644 test.txt
ls -l test.txt

# 5. Create a script
touch myscript.sh
echo "echo 'Hello from script'" > myscript.sh

# 6. Try to run it
./myscript.sh       # Error! No execute permission

# 7. Add execute permission
chmod 755 myscript.sh

# 8. Run it now
./myscript.sh       # Works!
```

## Exercise 4: Shared Project

```bash
# 1. Create shared folder (as your user with sudo)
sudo mkdir /shared_project

# 2. Create a group for the team
sudo groupadd team

# 3. Add yourself to the team
sudo usermod -aG team $(whoami)

# 4. Change folder ownership
sudo chown $(whoami):team /shared_project

# 5. Set permissions (owner and group can do everything)
sudo chmod 770 /shared_project

# 6. Check it
ls -ld /shared_project
```

