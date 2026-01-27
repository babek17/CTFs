# Bandit Level 5=> Level 6 | OverTheWire
## Description
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

* human-readable
* 1033 bytes in size
* not executable

## Analysis
After loggin in we can run the command `ls`to see what we have in this directory:
```bash
bandit5@bandit:~$ ls
inhere
bandit5@bandit:~$ cd inhere/
bandit5@bandit:~/inhere$ ls
maybehere00  maybehere03  maybehere06  maybehere09  maybehere12  maybehere15  maybehere18
maybehere01  maybehere04  maybehere07  maybehere10  maybehere13  maybehere16  maybehere19
maybehere02  maybehere05  maybehere08  maybehere11  maybehere14  maybehere17
```
## Solution
Apparently in this CTF we need to find the file which is **readible**, **1033 bytes in size**, and **not executable**. To find that file we can use `find` command with flags to specify the output:
```bash
bandit5@bandit:~/inhere$ find . -maxdepth 2 -type "f" -size 1033c
./maybehere07/.file2
```
Here
* `find .` => find in current folder
* `-maxdepth 2`=> maximum number of subfolders to search
* `-type "f"=> specifies that the output will contain only files not directories
* `-size 1033c`=> specifies the size of the file to be exactly 1033 bytes. If we write +1033c, that means files which size is more than 1033 bytes. If we write -1033c, that means files which size is less than 1033 bytes.

The output for this command is the following:
```bash
bandit5@bandit:~/inhere$ find . -maxdepth 2 -type "f" -size 1033c
./maybehere07/.file2
```
Now we find out the file we are looking for.

If we enter the `maybehere07` folder and run `ls -la` command we can find this output:
```bash
bandit5@bandit:~/inhere/maybehere07$ ls -la
total 56
drwxr-x---  2 root bandit5 4096 Oct 14 09:26 .
drwxr-x--- 22 root bandit5 4096 Oct 14 09:26 ..
-rwxr-x---  1 root bandit5 3663 Oct 14 09:26 -file1
-rwxr-x---  1 root bandit5 3065 Oct 14 09:26 .file1
-rw-r-----  1 root bandit5 2488 Oct 14 09:26 -file2
-rw-r-----  1 root bandit5 1033 Oct 14 09:26 .file2
-rwxr-x---  1 root bandit5 3362 Oct 14 09:26 -file3
-rwxr-x---  1 root bandit5 1997 Oct 14 09:26 .file3
-rwxr-x---  1 root bandit5 4130 Oct 14 09:26 spaces file1
-rw-r-----  1 root bandit5 9064 Oct 14 09:26 spaces file2
-rwxr-x---  1 root bandit5 1022 Oct 14 09:26 spaces file3
```
As we can observe, the `.file2` was a **hidden non-executable** file. This is also confirmed that we found out the flag correctly!!

```bash
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
HWasnPh-------------------
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        bandit5@bandit:~/inhere$ 
``` 
