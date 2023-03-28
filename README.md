# RSA Factoring (RSA Data Security and Cryptographic Algorithm)

**RSA** *(Rivest–Shamir–Adleman)* is a [Public-Key Cryptosystem/Cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) algorithm that is widely used for secure data transmission. The name **RSA** is an acronym from the surnames of the 3 inventors of this wonderful data security algorithm: **(*Ron Rivest*, *Adi Shamir*,** and ***Leonard Adleman*)** in the year 1973.

As regards the RSA algorithm is based on the public-key cryptosystem, the encryption key is public and distinct from the decryption key, which is kept secret (private). An RSA algorithm user creates and publishes a public key based on two large prime numbers, along with an auxiliary value. The prime numbers are kept secret. Messages can be encrypted by anyone, via the public key, but can only be decoded by someone who knows or has the prime numbers or private key.

The security of RSA relies on the practical difficulty of factoring the product of two large prime numbers, the "factoring problem".

## How RSA Data Security Algorithm is Generated and Used

The process of generating and using this algorithm is in 4 different stages:

- <a href="https://sampulcodemine.hashnode.dev/rsa-factoring#key-gen" target="_self">Key Generation</a>
    
- <a href="https://sampulcodemine.hashnode.dev/rsa-factoring#key-dist" target="_self">Key Distribution</a>
    
- <a href="https://sampulcodemine.hashnode.dev/rsa-factoring#enc" target="_self">Data Encryption</a>
    
- <a href="https://sampulcodemine.hashnode.dev/rsa-factoring#dec" target="_self">Data Decryption</a>
    

<h3 id="key-gen">Key Generation</h3>

Find below the steps to generate the keys

* Generate two (2) large prime numbers `p` and `q`. These prime numbers `p` and `q` should be chosen at random and different in length.
    

```bash
p = 41
q = 211
```

* Compute `n` such that `n = p * q`
    

```bash
p = 41 # first prime number
q = 211 # second prime number
n = p * q
```

* Compute the totient of n. `φ(n)` is equivalent to `(p - 1) * (q - 1)` and also equivalent to the `lcm(p-1, q-1)`
    

```bash
p = 41 # first prime number
q = 211 # second prime number
n = p * q  # product of p and q
m = (p - 1) * (q - 1) # totient of n

# note that m is equivalent to (p-1) * (q-1) 
# and also equivalent to the lcm of p and q [lcm(p-1, q-1)]
```

*Note: the totient has to be kept secret.*

* We have to choose an integer `e` such that 2 is less than `e` and `e` is less than totient of `n` and the *greatest common divisor* `gcd(e, φ(n))` = 1.  
    In a nutshell, `2 < e < φ(n)` and `e` is not a divisor of `φ(n)` and is a prime number. `e` is kept as our **public key exponent** used for encryption.
    

```bash
p = 41 # first prime number
q = 211 # second prime number
n = p * q  # product of p and q
m = (p - 1) * (q - 1) # totient of n

# note that m is equivalent to (p-1) * (q-1) 
# and also equivalent to the lcm of p and q [lcm(p-1, q-1)]

e = 113 # let us choose e to be 113
```

* Using the modular multiplicative inverse of `e mod φ(n)` we can get the value of our **private key exponent** `d`.
    

```bash
p = 41 # first prime number
q = 211 # second prime number
n = p * q  # product of p and q
m = (p - 1) * (q - 1) # totient of n

# note that m is equivalent to (p-1) * (q-1) 
# and also equivalent to the lcm of p-1 and q-1 [lcm(p-1, q-1)]

e = 113 # let us choose e to be 113

# using the modular multiplicative inverse, 
d = 617
```

**Note:** *From the above calculations, our* ***public key*** *consists of* `n` and the encryption key `e` while the **private key or decryption key** is `d`. The variables `p`, `q`, `n`, and `d` should be kept secret.

A little program was written to give us work through how the calculation was done.

```python
import math

"""Funtion to calculate LCM"""
def get_lcm(arr):
    lcm = arr[0]
    for itr in range(1, len(arr)):
        lcm = lcm * arr[itr]//math.gcd(lcm, arr[itr])
    return lcm
 
"""Funtion to perform Modular Multiplicative Inverse"""
def mod_mul_inv(ele, tot):
    for itr in range(1, tot):
        if ((ele % tot) * (itr % tot) % tot) == 1:
            return itr
    return -1


"""Two prime numbers p and q chosen at random """
p = 41
q = 211
n = p * q 
totient_val = (p-1) * (q-1) # using totient of n
lcm_val = get_lcm([p - 1, q - 1]) # using the lcm of (p, q)

e = 113 # choose a prime integer value

# Getting the decryption key using the Totient for n
for i in range(1, totient_val):
    if ((e % totient_val) * (i % totient_val) % totient_val) == 1:
        print(i)


for j in range(1, lcm_val):
    if ((e % lcm_val) * (j % lcm_val) % lcm_val) == 1:
        print(j)
  

# This will output
#
# 8177 - if you are using the φ(n)= (p-1)(q-1)
# 
# or
# 
# 617 - if you are using the lcm((p-1), (q-1))
```

<h3 id="key-dist">Key Distribution</h3>

When Key generation is complete, key distribution has to do with the sender knowing the public key of the receiver which will be used to encrypt the message. The receiver then uses the private key to decrypt the encrypted message.

To enable the sender to encrypt and send the message to the receiver, the receiver transmits the public key (`n`, `e`) to the sender. When the receiver, receives the message, they use the private key (`d`) to decrypt the message.

<h3 id="enc">Data Encryption</h3>

When the sender receives the public key (`n`. `e`), he/she can send the message. He computes the ciphertext using the public key sent from the receiver.  

`ciphertext` = `plain_message`<sup>`e`</sup> (mod `n`)

The `ciphertext` will then be transmitted to the receiver.


<h3 id="dec">Data Decryption</h3>

The receiver upon receiving the `ciphertext` can recover the `plain_message` by using the private key `d`  

`plain_message` = `ciphertext`<sup>`d`</sup> (mod `n`)


**A Simple Computation**

`# Choosing 2 prime numbers`

`p` = 41

`q` = 211


`# get the product of p and q`

`n` = (`p`*`q`)

`n` = 41 * 211 = 8651


`# using totient of n`

`m` = (`p`-1)*(`q`-1)

`m` = (41 - 1)*(211 - 1)

`m` = 40*210
  
`m` = 8400

`# Choose a prime number greater than` 2 `but less than` `n` ` which will be a coprime with n`
 
`e` = 113 

`# from the program we wrote in the key generation session we can find the value of` `d`

`d` = 8177
  
`# for instance our plain_message is 65` 

`# to encrypt` 

`cipher_text` = `plain_message`<sup>`e`</sup> (mod `n`)
  
`cipher_text` = 65<sup>113</sup> (mod 8651)
  
`cipher_text` = 1047 

`# to decrypt` 

`plain_message` = `cipher_text`<sup>`d`</sup> (mod `n`)  

`plain_message` = 1047<sup>8177</sup> (mod 8651)
  
`plain_message` = 65


`# using `lcm(`p`-1, `q`-1)`

`m` = lcm(`p` - 1), (`q` - 1)
  
`m` = 840
  
`e` = 113
  
`d` = 617
  
`# for instance our plain_message is 65` 

`# to encrypt` 

`cipher_text` = `plain_message`<sup>`e`</sup> (mod `n`)
  
`cipher_text` = 65<sup>113</sup> (mod 8651) 
 
`cipher_text` = 1047 

`# to decrypt` 

`plain_message` = `cipher_text`<sup>d</sup> (mod `n`)
  
`plain_message` = 1047<sup>617</sup> (mod 8651)
  
`plain_message` = 65

---

> I think with these little examples given and the explanations, you could research with more advance materials, but I guess you have a clear concepts of the workings of RSA Cryptographic Data Security.