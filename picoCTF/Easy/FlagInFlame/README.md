# Flag in Flame – CTF Write-up

## Challenge Overview
In this challenge we are given a large logs.txt file with not understandable text. We need to find out what is it and how to use it.

## Analysis
At first I tried to run command `grep` to see if this file contains `picoCTF`. It did not work. Then I tried to decode it using ROT13 and run `grep` command again. Still not successful. I tried to decode it using Base64 and found some interesting output. It was a long confusing text, however, it had **`PNG`** at the top.

## Converting
After seeing PNG on the top of output, I decided to convert this base64 decoded output into image. I used CyberChef again and it did not let me down. I got some image with person in hoodie with some text on the bottom of the picture. I searched for tools and decided to use tool called **tesseract** to extract the text from image.

## Second Analysis
After extracting that text, i run it through popular encoding methods like rot13, base64. They did not work. Then I run it through "from hex" and got the flag for current ctf!
