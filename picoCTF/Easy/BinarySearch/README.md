# Binary Search | picoCTF

## Analysis

After connecting to the machine using ssh, a game with binary search starts. After we enter an input, it tells us if chosen number is higher/lower than our input. Our goal is to find that chosen number in 10 guesses. Otherwise, the connection will be terminated.

## Solution

In binary search with higher/lower, we need to use simple formula: 
```bash
guess the middle => guess = (low + high) // 2

Get feedback

If the guess is too low → the number is higher → move low up: low = guess + 1

If the guess is too high → the number is lower → move high down: high = guess - 1

If it’s correct → you’re done!
```

## Answer

Using the method above, we can complete this CTF:

```bash
┌──(kali㉿kali)-[~]
└─$ ssh -p 52581 ctf-player@atlas.picoctf.net
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
ctf-player@atlas.picoctf.net's password: 
Welcome to the Binary Search Game!
I'm thinking of a number between 1 and 1000.
Enter your guess: 500
Higher! Try again.
Enter your guess: 750
Lower! Try again.
Enter your guess: 625
Lower! Try again.
Enter your guess: 562
Lower! Try again.
Enter your guess: 531
Higher! Try again.
Enter your guess: 546
Higher! Try again.
Enter your guess: 554
Congratulations! You guessed the correct number: 554
Here's your flag: picoCTF{-----------------}
Connection to atlas.picoctf.net closed.
```
