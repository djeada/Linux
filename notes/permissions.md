## Permissions

| Permission | Files | Dirs |
| --- | --- | --- |
| read | The file's contents can be seen by the user | The files in the directory can be listed by the user |
| write | The file's contents can be changed by the user | The user can add new files to the directory and delete old ones |
| execute | The filename can be used as a UNIX command by the user | The user can go to the directory, but they cannot list the files unless they have read permission. |

Permissions may be specified symbolically, using the symbols u (user), g (group), o (other), a (all), r (read), w (write), x (execute), + (add permission), - (take away permission) and = (assign permission).

For example, the following command will grant the file's owner execution permission:

```bash
chmod u+x file
```

| Permissions | Number |
| --- | --- |
| --- | 0 |
| --x | 1 |
| -w- | 2 |
| -wx | 3 |
| r-- | 4 |
| r-x | 5 |
| rw- | 6 |
| rwx | 7 |

## Default permissions

* Files: rw-rw-rw- (666)
* Directories: rwxrwxrwx (777)

Use <code>umask</code> to change the default permissions. With no options specified, <code>umask</code> displays which current default permissions are removed (masked).
Three numbers, for user, group and others.

For example, let's say we want to:

1. prevent the file's owner (user) from being granted the execute permission while leaving the rest of the owner permissions untouched;
2. allow the group to read while restricting the group from writing or executing;
3. allow write permission for others while not changing the other permissions.

Then, we would use:

```bash
umask u-x,g=r,o+w
```

## Setuid, Setgid, and the Sticky Bit

* <code>setuid</code>: a bit that causes an executable to execute with the file's owner's privileges.
* <code>setgid</code>: a bit that causes an executable to execute with the privileges of the file's group.
* <code>sticky bit</code>: a directory bit that permits only the owner or root to remove files and subdirectories. 

To set the <code>setuid</code> bit, use:

```bash
chmod u+s /path/to/file
```
To remove the <code>setuid</code> bit:

```bash
chmod u-s /path/to/file
```
To set the <code>setgid</code> bit:

```bash
chmod g+s /path/to/dir
```
To remove the <code>setgid</code> bit:

```bash
chmod g-s /path/to/dir
```
To set the <code>sticky bit</code>, use:

```bash
chmod +t /path/to/dir
```
To remove the <code>sticky bit</code>, use:

```bash
chmod -t /path/to/dir
```

## ACls
ACLs (access control lists) in Linux are discretionary access control system permissions that are built on top of regular Linux permissions.

* not all tools support ACLs
* a modern mke2fs now sets acl in default mount options automatically at filesystem creation time, at least in "enterprise" Linux distributions

Set (replaces), modify, or remove the access control list using the <code>setfacl</code> command (ACL). It also updates and deletes ACL entries for each path-specified file and directory. If no path is given, the names of files and directories are taken from standard input (stdin). In this scenario, each line of input should have one path name.

* use <code>-m</code> flag to modify the ACLs:

```bash
setfacl -m g:group_name:rw /opt/test
```

* to have all new files in the directory inherit the ACLs, use the <code>-m</code> flag with the <code>d</code> option:

```bash
setfacl -m d:g:group_name:rw /opt/test
```

* use <code>-X</code> lag to remove a user or group:

```bash
setfacl -X g:group_name /opt/test
```

* use <code>-b</code> flag to remove everything: 

```bash
setfacl -b /opt/test
```

* use <code>-k</code> flag to go back to default ACL's: 

```bash
setfacl -k /opt/test
```

* ACLs from one file/dir can be reused in another one:

```bash
getfacl /opt/test | setfacl --set-file= /opt/test2
```

## Challenges

1. Make a temporary text file named temp.txt in your home directory. Using the <code>ls -l</code> command, check the permissions. You'll probably see something like this: 

```bash
-rw-rw-r-- 1 user_name user_group  8 Nov 21 18:02 temp.txt
```

As a result, the file is owned by the user "user name" and the group "user group," who are the only ones who can write to it - but any other user may read it.

Now remove the "user group" group's permission to write to the file and read permission from others.

2. Copy a root-owned file from /etc/ to your home directory; who now owns this file? 
3. Show the umask of any file in both octal and symbolic form.
4. What happens when the owner of a file does not have the necessary permissions to interact with it? Can he, at the very least, remove such a file? 
5. Explain what happens when you try to remove the group's permission to write to the file and read permission from others.
6. Explain the difference between permissions and ACLs.
7. Can a user who is not the owner of a file or directory change the permissions of the file or directory?
8. Should you use ACLs or permissions to restrict file and directory access? 
