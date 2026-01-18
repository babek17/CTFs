# Head-dump | picoCTF

We are given a website which should contain endpoints that will help us to retrieve flag.

## Analysis

After entering the website we see several sections like "Home", "About", "Services". Also, we see several posts and one of them is about API Documentation which might help us. Upon pressing that blog, we are redirected to Swagger page with endpoints. On the bottom we see GET endpoint saying "/heapdump
Diagnosing the memory allocation." I pressed try and then execute. I got a .heapsnapshot file!

## Answer
After receiving .heapsnapshot file, I tested it using `file` command I got the following output:
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ file heapdump-1768772852203.heapsnapshot 
heapdump-1768772852203.heapsnapshot: ASCII text, with very long lines (64680)
```
So all I did is to use `grep` command to retrieve the flag:
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ grep picoCTF heapdump-1768772852203.heapsnapshot
picoCTF{------------------------}
"strings":["<dummy>","","(GC roots)","(Bootstrapper)","(Builtins)","(Client heap)","(Code flusher)","(Compilation cache)","(Debugger)","(Ext
```

We got the flag!
