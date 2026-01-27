# Bandit Level 1=>Level2 | OverTheWire
## Description
The password for the next level is stored in a file called - located in the home directory.
## Analysis
After connecting to the machine using `ssh`, we observe the following:
```bash
bandit1@bandit:~$ ls
-
```
## Solution
The `cat` command treats `-` symbol as special symbol. In Unix-bases systems `cat` with combination of `-` command expect standard input rather reading from file name. That is why we need to change code a little.

## Answer
Now, all we have to do is to run `cat` command using `./-`:
```bash
bandit1@bandit:~$ cat ./-
----------------------------
```
Now `cat` command treats `-` as a filename  and we can read the password for bandit2.


