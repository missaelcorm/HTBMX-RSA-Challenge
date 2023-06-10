# Resources:
![hacker.jpg](hacker.jpg)
```
e = 65537
m = 841369317301357750365699129721153968
N = 1034776851837418228051242693253376923
```

# Decrypting

First we ask to ChatGPT about the given variables.

Input
```
encryption method using variables like n, m and e
```

Output/ChatGPT Answer
```
One popular encryption method that uses variables like n, m, and e is the RSA (Rivest-Shamir-Adleman) algorithm. RSA is a widely used public-key encryption scheme that allows secure communication over an insecure network.

Here's a brief overview of how RSA encryption works:

Key Generation:

Choose two large prime numbers, p and q.
Calculate n = p * q. This is the modulus used for both encryption and decryption.
Calculate Euler's totient function, φ(n) = (p-1) * (q-1).
Choose an integer e such that 1 < e < φ(n), and e is coprime with φ(n) (i.e., they have no common factors other than 1). This is the public exponent.
Calculate the modular multiplicative inverse of e modulo φ(n) and call it d. This is the private exponent.
Encryption:

Convert the plaintext message m into a numerical representation (e.g., using ASCII or Unicode).
Encrypt the message by computing c = m^e mod n, where c is the ciphertext.
Decryption:

Decrypt the ciphertext by computing m = c^d mod n, where m is the decrypted message.
In this process, n is the product of two large prime numbers and serves as the modulus for encryption and decryption operations. e is the public exponent used for encryption, and d is the private exponent used for decryption.

The security of RSA relies on the difficulty of factoring the modulus n into its prime factors p and q. The larger the prime numbers used, the harder it becomes to break the encryption.

It's worth noting that while this explanation provides a general overview, the actual implementation of RSA involves additional steps and considerations, such as padding and hashing, to ensure security and address specific cryptographic requirements.
```

So now we know that we are talking about an RSA encryption.

Looking around the web, I found a tool than can help us with the decryption.
The tools is [RsaCtfTool](https://github.com/RsaCtfTool/RsaCtfTool)

Executing `RsaCtfTool`
```bash
python3 RsaCtfTool.py -n 1034776851837418228051242693253376923 -e 65537 --uncipher 841369317301357750365699129721153968
```

And we get the uncyphered data:
```
Unciphered data :
HEX : 0x000000000000061999dc52989d50b4
INT (big endian) : 112525212211948114100
INT (little endian) : 936248513267971001104460644572200960
STR : b'\x00\x00\x00\x00\x00\x00\x06\x19\x99\xdcR\x98\x9dP\xb4'
```

We got focus on `INT (big endian) : 112525212211948114100`

Now our Best Friend is gonna be the ASCII Table, and splitting that number in this way 
```
112 52 52 122 119 48 114 100
```
we get a string: `p44sw0rd`
```
112 -> p
52  -> 4
52  -> 4
122 -> s
119 -> w
48  -> 0
114 -> r
100 -> d
```

Then using `steghide` over the image provided, we can see that string is not the password, so we are FUCK.

I tried a lot of encodings/decodings but nothing worked, my ideas are running out...