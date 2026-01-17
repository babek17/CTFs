# Log Hunt – CTF Write-up

## Challenge Overview
In this challenge we are provided with a `server.log` file. The goal is to locate and reconstruct a flag that is hidden within the log output.

## Approach
After briefly examining the file, it becomes clear that only specific log entries contain fragments of the final flag. These lines are consistently labeled with the log level and tag:

`INFO FLAGPART`

To efficiently extract only the relevant log entries, we can use standard Linux command-line tools. In particular, `grep` allows us to filter output based on a search pattern.

## Command Used
```bash
cat ~/Downloads/server.log | grep "INFO FLAGPART"
```
Now all we need to do is to combine all these parts!
