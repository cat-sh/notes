# Permissions on Linux Systems

The output of `ls -l` can be divided into 6 sections.

```
$ ls -l
total 520
-rw-r--r--. 1 cfs cfs  20081 Mar 25 21:58 chapter-1.html
-rw-r--r--. 1 cfs cfs     80 Mar 25 11:27 grep.txt
-rw-r--r--. 1 cfs cfs    177 Mar 25 13:56 java.txt
drwxr-xr-x. 1 cfs cfs     22 Mar 24 23:37 lapio
```

#### 1st Column 

The first letter is either - or d, which tells if the resource is a file - or a directory d

r - read, w - write, x - execute

1. First rwx group describes **owner** permissions
2. Second group describes **group** permissions
3. Third group describes **other users** permissions (those who are not the owner, or belong to the group of the file)

#### 2nd Column

After permissions, one can see the number of links pointing to a file. In folders, this number represents the number of files inside.

#### 3rd Column

Owner and group the file belongs to. 

#### 4th Column 

File size in bytes.

#### 5th Column 

Date the file was last modified.

#### 6th Column

Name of the file/folder.

## Change Permissions

Permissions can be changed with `chmod`(_change mode_).

`-` removes permissions
`+` adds permissions

```
r = read permission
w = write permission
x = execute permission
u = owner of the file
g = users belonging to the group of the file
o = all other users
```

If the user is not defined as an argument, permissions are given to all user groups. 

`chmod u+x example.txt` gives the owner of the file execute permission.

`chmod o-w example.txt` removes other users the write permission.


---

### Sources

[University of Helsinki](https://courses.mooc.fi/org/uh-cs/courses/computing-tools-for-cs-studies/chapter-1#dcf75d0f-3494-44e6-ae9f-fba4a1b6d7fb)

Read more on Unix groups and group ownership [here](https://csguide.cs.princeton.edu/account/groups)
