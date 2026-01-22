# Rust Fixme 3 | picoCTF
## Analysis
We are given rust code with problems, which we need to solve.

## With help of Chatgpt I found out that this code had following issues:

1) The function needed to modify a string passed in from main.
2) Missing semicolons and improper return statements.
3) Accessing decrypted data via raw pointers required an unsafe block.


## Solution
After solving those problems we get the flag for this CTF:

picoCTF{n0w_y0uv3_f1x3d_1h3m_411}




