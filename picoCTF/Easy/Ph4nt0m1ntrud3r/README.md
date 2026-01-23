# RED | picoCTF

## Analysis

After downloading .pcap file, I opened `Wireshark` to check the packages and find something interesting. After checking all packages, I saw that the endings of each package had some string, which was very similar to Base64 encoding scheme. I used `tshark` to make it more visual.
This was the output:
```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ tshark -r myNetworkTraffic.pcap -x
0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 05 c6 00 00 36 77 44 6f 54 38 38 3d   P. .....6wDoT88=

0000  45 00 00 34 00 01 00 00 40 06 f8 6e c0 a8 00 02   E..4....@..n....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 eb 52 00 00 62 6e 52 66 64 47 67 30   P. ..R..bnRfdGg0
0030  64 41 3d 3d                                       dA==

0000  45 00 00 2c 00 01 00 00 40 06 f8 76 c0 a8 00 02   E..,....@..v....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 69 97 00 00 66 51 3d 3d               P. .i...fQ==

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 05 f5 00 00 2b 54 48 32 52 69 41 3d   P. .....+TH2RiA=

0000  45 00 00 34 00 01 00 00 40 06 f8 6e c0 a8 00 02   E..4....@..n....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 46 0b 00 00 5a 54 46 6d 5a 6a 41 32   P. .F...ZTFmZjA2
0030  4d 77 3d 3d                                       Mw==

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 fa b5 00 00 4b 69 5a 50 2b 75 41 3d   P. .....KiZP+uA=

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 eb b1 00 00 6d 55 4b 6c 34 71 34 3d   P. .....mUKl4q4=

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 b8 ff 00 00 42 62 7a 51 67 31 30 3d   P. .....BbzQg10=

0000  45 00 00 34 00 01 00 00 40 06 f8 6e c0 a8 00 02   E..4....@..n....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 f6 5a 00 00 58 7a 4d 30 63 33 6c 66   P. ..Z..XzM0c3lf
0030  64 41 3d 3d                                       dA==

0000  45 00 00 34 00 01 00 00 40 06 f8 6e c0 a8 00 02   E..4....@..n....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 05 5b 00 00 65 7a 46 30 58 33 63 30   P. ..[..ezF0X3c0
0030  63 77 3d 3d                                       cw==

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 b4 cb 00 00 51 6a 39 61 74 4d 59 3d   P. .....Qj9atMY=

0000  45 00 00 34 00 01 00 00 40 06 f8 6e c0 a8 00 02   E..4....@..n....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 fd 41 00 00 63 47 6c 6a 62 30 4e 55   P. ..A..cGljb0NU
0030  52 67 3d 3d                                       Rg==

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 d0 cb 00 00 55 4a 64 64 4e 6a 34 3d   P. .....UJddNj4=

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 61 24 00 00 75 31 6e 37 61 57 67 3d   P. .a$..u1n7aWg=

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 d7 b2 00 00 6f 6b 4b 54 42 72 38 3d   P. .....okKTBr8=

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 bc 01 00 00 5a 44 51 6b 50 33 55 3d   P. .....ZDQkP3U=

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 aa ce 00 00 55 42 63 5a 50 79 59 3d   P. .....UBcZPyY=

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 aa 18 00 00 79 38 38 2f 70 64 41 3d   P. .....y88/pdA=

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 b2 ae 00 00 4d 77 62 4d 59 71 51 3d   P. .....MwbMYqQ=

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 e4 03 00 00 2b 52 56 38 4e 56 59 3d   P. .....+RV8NVY=

0000  45 00 00 34 00 01 00 00 40 06 f8 6e c0 a8 00 02   E..4....@..n....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 27 f7 00 00 59 6d 68 66 4e 48 4a 66   P. .'...YmhfNHJf
0030  4d 67 3d 3d                                       Mg==

0000  45 00 00 30 00 01 00 00 40 06 f8 72 c0 a8 00 02   E..0....@..r....
0010  c0 a8 01 02 00 14 00 50 00 00 00 00 00 00 00 00   .......P........
0020  50 02 20 00 f4 85 00 00 6c 77 46 70 35 77 30 3d   P. .....lwFp5w0=
```

Afterwards, I tried to "puzzle" with these strings, because some of the worked and some of them did not. Some time later I had the following pieces:
```bash
    cGljb0NURg== picoCTF

    ezF0X3c0cw== {1t_w4s

    MzE4ZGIyMg== 318db22

    XzM0c3lfdA== _34sy_t

    YmhfNHJfZg== bh_4r_f

    bnRfdGg0dA== nt_th4t

    fQ= }
```
## Solution

Combining these pieces gave me the correct answer, however, it took too long to do this. I used AI to find faster way to extract the data from this, and figured out the following command:

```bash
tshark -r myNetworkTraffic.pcap -Y "tcp.len==12 || tcp.len==4" \
  -T fields -e frame.time -e tcp.segment_data \
| sort \
| awk -F'\t' '{print $2}' \
| xxd -p -r \
| base64 -d
```
In this command:

1️⃣ `tshark -r myNetworkTraffic.pcap` => runs tshark and uses file as an input instead live capturing

2️⃣ `-Y "tcp.len==12 || tcp.len==4"` => filters packets by their TCP payload length, 12 and 4 in our case

3️⃣ `-T fields` => this commands help us to select specific fields instead of selecting full packet. Without this command -e would not work.

4️⃣ `-e frame.time -e tcp.segment_data` => frame.time is used to have human-readable timestamp and preserving the order of packets. tcp.segment_data is the raw tcp payload in hex format.

5️⃣ `| sort` => since tshark does not guarantee strict time order, we are ensuring ourselves with this command

6️⃣ `| awk -F'\t' '{print $2}'` => `-F'\t'` → split on TAB, `$2` → second field = `tcp.segment_data`. This strips away timestamp and leaves only pure hex string.

7️⃣ `| xxd -p -r` => `-p` = plain hex (no offsets, no formatting), `-r` = reverse operation (hex → binary). Without this step base64 would try to decode ASCII hex characters

8️⃣ `| base64 -d` => Interprets the raw byte stream as base64. Outputs decoded content (text or binary).

Using this command gives us flag for current CTF!

```bash
┌──(kali㉿kali)-[~/Downloads]
└─$ tshark -r myNetworkTraffic.pcap -Y "tcp.len==12 || tcp.len==4" \
  -T fields -e frame.time -e tcp.segment_data \
| sort \
| awk -F'\t' '{print $2}' \
| xxd -p -r \
| base64 -d
picoCTF{-----------------------------}
```
