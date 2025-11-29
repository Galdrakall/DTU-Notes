
This note gives **small, fully worked RSA examples** so you can see every step:

- Example 1: Tiny RSA (easy arithmetic)
- Example 2: Slightly larger example using square-and-multiply
- Example 3: RSA as a signature scheme

These are NOT secure sizes, just for learning.

---

## Example 1 – Tiny RSA (KeyGen, Encrypt, Decrypt)

We construct RSA with very small primes:

### 1. Choose primes

Let
- $p = 11$
- $q = 19$

Compute the modulus:
$$
N = p \cdot q = 11 \cdot 19 = 209
$$

Compute Euler’s totient:
$$
\varphi(N) = (p-1)(q-1) = 10 \cdot 18 = 180
$$

So:
- $N = 209$
- $\varphi(N) = 180$

---

### 2. Choose public exponent $e$

We need $e$ such that $\gcd(e, \varphi(N)) = 1$.

Take:
- $e = 7$

Check:
- $\gcd(7, 180) = 1$ → OK.

---

### 3. Compute private exponent $d$

We need $d$ such that:
$$
e \cdot d \equiv 1 \pmod{\varphi(N)}
$$
i.e.
$$
7d \equiv 1 \pmod{180}
$$

Try $d = 103$:

- $7 \cdot 103 = 721$
- $721 \div 180 = 4$ remainder $1$  
  (since $4 \cdot 180 = 720$ and $721 - 720 = 1$)

So:
$$
7 \cdot 103 \equiv 1 \pmod{180}
$$

Thus:
- Public key: $(N, e) = (209, 7)$
- Private key: $d = 103$

---

### 4. Encrypt a message

Let the plaintext be
- $m = 42$ (must be $< N$)

Encryption:
$$
c = m^e \bmod N = 42^7 \bmod 209
$$

Compute with repeated squaring:

1. $42^1 \bmod 209 = 42$
2. $42^2 = 42 \cdot 42 = 1764$  
   Reduce mod 209:  
   $209 \cdot 8 = 1672$  
   $1764 - 1672 = 92$  
   So $42^2 \equiv 92 \pmod{209}$
3. $42^4 = (42^2)^2 \equiv 92^2 \pmod{209}$  
   $92^2 = 8464$  
   $209 \cdot 40 = 8360$  
   $8464 - 8360 = 104$  
   So $42^4 \equiv 104 \pmod{209}$
4. $42^7 = 42^{4+2+1} = 42^4 \cdot 42^2 \cdot 42^1$  

   Multiply step by step:

   - First $42^4 \cdot 42^2 \equiv 104 \cdot 92$  
     $104 \cdot 92 = 9568$  
     $209 \cdot 45 = 9405$  
     $9568 - 9405 = 163$  
     So this product $\equiv 163 \pmod{209}$  

   - Now multiply by $42^1$:  
     $163 \cdot 42 = 6846$  
     $209 \cdot 32 = 6688$  
     $6846 - 6688 = 158$  

So:
$$
c = 42^7 \bmod 209 = 158
$$

Ciphertext:
- $c = 158$

---

### 5. Decrypt the ciphertext

Decryption:
$$
m' = c^d \bmod N = 158^{103} \bmod 209
$$

The arithmetic is long by hand, but the theory says:
$$
m' = m = 42
$$

You can check with a calculator or computer that:
- $158^{103} \bmod 209 = 42$

So decryption recovers the original plaintext.

---

## Example 2 – Larger (but still toy) RSA with Square-and-Multiply

This is a classic textbook example with slightly bigger numbers.

### 1. Choose primes

Let
- $p = 61$
- $q = 53$

Compute:
$$
N = p \cdot q = 61 \cdot 53 = 3233
$$
$$
\varphi(N) = (p-1)(q-1) = 60 \cdot 52 = 3120
$$

---

### 2. Choose $e$

Pick:
- $e = 17$

Check:
- $\gcd(17, 3120) = 1$ → OK.

---

### 3. Compute $d$

Find $d$ such that:
$$
17d \equiv 1 \pmod{3120}
$$

The solution is:
- $d = 2753$

Check:
- $17 \cdot 2753 = 46801$
- $3120 \cdot 15 = 46800$
- $46801 - 46800 = 1$

So:
$$
17 \cdot 2753 \equiv 1 \pmod{3120}
$$

Keys:
- Public: $(N, e) = (3233, 17)$
- Private: $d = 2753$

---

### 4. Encrypt

Let message:
- $m = 65$

Encrypt:
$$
c = m^e \bmod N = 65^{17} \bmod 3233
$$

Use square-and-multiply.

First compute powers of $65$ by squaring:

1. $65^1 \bmod 3233 = 65$
2. $65^2 = 65 \cdot 65 = 4225$  
   $4225 - 3233 = 992$  
   So $65^2 \equiv 992 \pmod{3233}$
3. $65^4 \equiv 992^2 \pmod{3233}$  
   $992^2 = 984064$  
   $3233 \cdot 300 = 969900$  
   $984064 - 969900 = 14164$  
   $3233 \cdot 4 = 12932$  
   $14164 - 12932 = 1232$  
   So $65^4 \equiv 1232 \pmod{3233}$
4. $65^8 \equiv 1232^2 \pmod{3233}$  
   $1232^2 = 1517824$  
   $3233 \cdot 400 = 1\,293\,200$  
   $1517824 - 1293200 = 224624$  
   $3233 \cdot 60 = 193980$  
   $224624 - 193980 = 30644$  
   $3233 \cdot 9 = 29097$  
   $30644 - 29097 = 1547$  
   So $65^8 \equiv 1547 \pmod{3233}$
5. $65^{16} \equiv 1547^2 \pmod{3233}$  
   $1547^2 = 2393209$  
   $3233 \cdot 700 = 2\,263\,100$  
   $2393209 - 2263100 = 130109$  
   $3233 \cdot 40 = 129320$  
   $130109 - 129320 = 789$  
   So $65^{16} \equiv 789 \pmod{3233}$

Now:
- $17 = 16 + 1$

So:
$$
65^{17} = 65^{16} \cdot 65^1
$$

Compute:
- $65^{16} \equiv 789$
- $65^1 = 65$

Multiply:
- $789 \cdot 65 = 789 \cdot (60 + 5) = 789 \cdot 60 + 789 \cdot 5$
- $789 \cdot 60 = 47\,340$
- $789 \cdot 5 = 3\,945$
- Sum: $47\,340 + 3\,945 = 51\,285$

Reduce mod 3233:
- $3233 \cdot 15 = 48\,495$
- $51\,285 - 48\,495 = 2\,790$

So:
$$
c = 65^{17} \bmod 3233 = 2790
$$

Ciphertext:
- $c = 2790$

---

### 5. Decrypt

Now decrypt:
$$
m' = c^d \bmod N = 2790^{2753} \bmod 3233
$$

The arithmetic is long by hand, but you can check with a computer that:
$$
m' = 65
$$

This confirms correctness:
$$
\text{Dec}_{sk}(\text{Enc}_{pk}(65)) = 65
$$

---

## Example 3 – RSA as a Signature Scheme (Tiny Example)

We reuse the **tiny RSA** from Example 1:

- $N = 209$
- $e = 7$
- $d = 103$

We’ll “sign” a small message:
- $m = 42$

Here, think of $m$ as a hash of a real message.

---

### 1. Signing

A signature is created with the **private key**:
$$
s = m^d \bmod N
$$

Compute:
$$
s = 42^{103} \bmod 209
$$

(You can do this with repeated squaring or a calculator.)

The result is:
- $s = 47$

So the signature on message $m=42$ is:
- $s = 47$

---

### 2. Verification

Anyone with the **public key** $(N, e)$ can verify a signature by checking:
$$
m \stackrel{?}{=} s^e \bmod N
$$

Compute:
$$
s^e = 47^7 \bmod 209
$$

You can do this similarly with repeated squaring or a calculator; the result is:
- $47^7 \bmod 209 = 42$

So:
- $s^e \bmod N = 42 = m$

The signature is valid.

---

## Summary

- RSA key generation uses:
  - $N = pq$
  - $\varphi(N) = (p-1)(q-1)$
  - $ed \equiv 1 \pmod{\varphi(N)}$
- Encryption: $c = m^e \bmod N$
- Decryption: $m = c^d \bmod N$
- Signatures flip the roles of $e$ and $d$:
  - Sign: $s = m^d \bmod N$
  - Verify: $m \stackrel{?}{=} s^e \bmod N$
- Square-and-multiply lets you compute $m^e \bmod N$ efficiently.

For exams, you should be comfortable:
- Computing $N$, $\varphi(N)$
- Finding $d$ given $e$ and $\varphi(N)$ (modular inverse)
- Doing small modular exponentiations by hand.
