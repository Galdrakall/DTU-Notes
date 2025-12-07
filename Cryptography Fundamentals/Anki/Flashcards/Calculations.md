TARGET DECK: Cryptography-Calculations

START
Basic
Compute the OTP ciphertext for m = 101101 and k = 011011. // Question
Back: c = m ⊕ k = 101101 ⊕ 011011 = 110110. // Answer
Tags: Calculation Week1 OTP
<!--ID: 1764867990886-->
END

START
Basic
Recover the OTP plaintext if c = 110110 and k = 011011. // Question
Back: m = c ⊕ k = 110110 ⊕ 011011 = 101101. // Answer
Tags: Calculation Week1 OTP
<!--ID: 1764867990890-->
END

START
Basic
Two OTP ciphertexts are c1 = 110010 and c2 = 001110 from the same key. Compute m1 ⊕ m2. // Question
Back: m1 ⊕ m2 = c1 ⊕ c2 = 110010 ⊕ 001110 = 111100. // Answer
Tags: Calculation Week1 OTP Reuse
<!--ID: 1764867990892-->
END

START
Basic
Given hash output length λ = 128, estimate the number of queries needed for a collision attack with non-negligible success. // Question
Back: About 2^64 queries, by the birthday bound. // Answer
Tags: Calculation Week6 Birthday
<!--ID: 1764867990894-->
END

START
Basic
A brute-force preimage attack on a 256-bit hash has success probability 2^{-256} per trial. How many trials are needed on average? // Question
Back: About 2^{256} trials on average (expected number of trials = 1/p). // Answer
Tags: Calculation Week6 Preimage
<!--ID: 1764867990897-->
END

START
Basic
Compute 7^4 mod 11. // Question
Back: 7^2 = 49 ≡ 5 (mod 11), so 7^4 = 5^2 = 25 ≡ 3 (mod 11). // Answer
Tags: Calculation Week7 ModularExponentiation
<!--ID: 1764867990899-->
END

START
Basic
RSA key generation: if p = 11 and q = 13, compute N and φ(N). // Question
Back: N = p·q = 11·13 = 143, φ(N) = (p−1)(q−1) = 10·12 = 120. // Answer
Tags: Calculation Week7 RSA
<!--ID: 1764867990901-->
END

START
Basic
For RSA with φ(N) = 120 and e = 7, compute a valid private exponent d. // Question
Back: d = 103, since 7·103 = 721 ≡ 1 (mod 120). // Answer
Tags: Calculation Week7 RSA
<!--ID: 1764867990903-->
END

START
Basic
Encrypt message m = 9 using RSA with public key (N = 143, e = 7). // Question
Back: c = 9^7 mod 143 = 4782969 mod 143 = 48. // Answer
Tags: Calculation Week7 RSA
<!--ID: 1764867990905-->
END

START
Basic
Decrypt RSA ciphertext c = 48 using private key d = 103 and N = 143. // Question
Back: m = 48^{103} mod 143 = 9. // Answer
Tags: Calculation Week7 RSA
<!--ID: 1764867990908-->
END

START
Basic
Compute 3^5 mod 17. // Question
Back: 3^2 = 9, 3^4 = 81 ≡ 13 (mod 17), so 3^5 = 3·13 = 39 ≡ 5 (mod 17). // Answer
Tags: Calculation Week8 DLP
<!--ID: 1764867990910-->
END

START
Basic
Diffie–Hellman: Let p = 23, g = 5, Alice’s secret a = 6. Compute Alice’s public value A. // Question
Back: A = g^a mod p = 5^6 mod 23 = 15625 mod 23 = 8. // Answer
Tags: Calculation Week10 DH
<!--ID: 1764867990912-->
END

START
Basic
Diffie–Hellman: Let p = 23, g = 5, Bob’s secret b = 15. Compute Bob’s public value B. // Question
Back: B = g^b mod p = 5^{15} mod 23 = 19. // Answer
Tags: Calculation Week10 DH
<!--ID: 1764867990915-->
END

START
Basic
Diffie–Hellman: With A = 8, B = 19, a = 6, p = 23, compute the shared key K from Alice’s side. // Question
Back: K = B^a mod p = 19^6 mod 23 = 2. // Answer
Tags: Calculation Week10 DH
<!--ID: 1764867990917-->
END

START
Basic
ElGamal keygen: Let p = 23, g = 5, secret key x = 6. Compute public key h. // Question
Back: h = g^x mod p = 5^6 mod 23 = 8. // Answer
Tags: Calculation Week8 ElGamal
<!--ID: 1764867990920-->
END

START
Basic
ElGamal encryption: Encrypt m = 7 with r = 10, p = 23, g = 5. Compute c0. // Question
Back: c0 = g^r mod p = 5^{10} mod 23 = 9. // Answer
Tags: Calculation Week8 ElGamal
<!--ID: 1764867990923-->
END

START
Basic
ElGamal encryption: With m = 7, h = 8, r = 10, p = 23, compute c1. // Question
Back: c1 = m·h^r mod p = 7·8^{10} mod 23 = 7·3 = 21 (mod 23). // Answer
Tags: Calculation Week8 ElGamal
<!--ID: 1764867990927-->
END

START
Basic
ElGamal decryption: Given c0 = 9, c1 = 21, secret x = 6, p = 23, recover m. // Question
Back: s = c0^x = 9^6 ≡ 3 (mod 23), s^{-1} ≡ 8, m = c1·s^{-1} ≡ 21·8 = 168 ≡ 7 (mod 23). // Answer
Tags: Calculation Week8 ElGamal
<!--ID: 1764867990931-->
END

START
Basic
CTR mode: Let nonce||counter encode to input value 7, and suppose E_k(7) = 11001100. What is the keystream block? // Question
Back: Keystream block = 11001100. // Answer
Tags: Calculation Week4 CTR
<!--ID: 1764867990936-->
END

START
Basic
CTR mode: With keystream = 11001100 and plaintext block m = 10101010, compute ciphertext block c. // Question
Back: c = m ⊕ keystream = 10101010 ⊕ 11001100 = 01100110. // Answer
Tags: Calculation Week4 CTR
<!--ID: 1764867990940-->
END

START
Basic
CBC-MAC: Suppose a message consists of two blocks. The block cipher outputs y1 = 0110 and y2 = 1101 after chaining. What is the MAC tag? // Question
Back: The CBC-MAC tag is the last block: 1101. // Answer
Tags: Calculation Week5 CBCMAC
<!--ID: 1764867990947-->
END

START
Basic
For a 64-bit block cipher, approximately how many random plaintext–ciphertext pairs are needed to expect a collision in the block space? // Question
Back: About 2^{32} pairs, by the birthday bound. // Answer
Tags: Calculation Week3 Birthday
<!--ID: 1764867990950-->
END

START
Basic
Elliptic curve over F_11 defined by y^2 = x^3 + x + 6 (mod 11). Given point P = (2,4), compute 2P. // Question
Back: Using EC doubling formulas, 2P = (5,9) on this curve. // Answer
Tags: Calculation Week10 ECC
<!--ID: 1764867990953-->
END

START
Basic
On the elliptic curve y^2 = x^3 + x + 6 over F_11, with P = (2,4) and 2P = (5,9), compute 3P. // Question
Back: 3P = 2P + P = (5,9) + (2,4) = (8,8). // Answer
Tags: Calculation Week10 ECC
<!--ID: 1764867990956-->
END

START
Basic
Grover’s algorithm: How many quantum queries are required to brute-force a 128-bit symmetric key? // Question
Back: About 2^{64} quantum queries. // Answer
Tags: Calculation Week12 Grover
<!--ID: 1764867990959-->
END

START
Basic
Grover’s algorithm: How many quantum queries are required to brute-force a 256-bit symmetric key? // Question
Back: About 2^{128} quantum queries. // Answer
Tags: Calculation Week12 Grover
<!--ID: 1764867990962-->
END

START
Basic
Mosca’s theorem: If data must remain secret for x = 20 years and migration takes y = 10 years, for which z (years until a useful quantum computer) must migration start now? // Question
Back: If z < x + y = 30 years, migration must start immediately. // Answer
Tags: Calculation Week12 Mosca
<!--ID: 1764867990965-->
END

START
Basic
Scalar LWE example: Let modulus p = 11, A = 2, secret s = 3, noise e = 1. Compute b = As + e mod p. // Question
Back: b = 2·3 + 1 = 7 mod 11. // Answer
Tags: Calculation Week8 LWE
<!--ID: 1764867990967-->
END

START
Basic
Vector LWE example: Let p = 11, A = [[1,2],[3,4]], s = [5,7]^T, e = [1,0]^T. Compute b = A·s + e mod p. // Question
Back: A·s = [1·5 + 2·7, 3·5 + 4·7]^T = [19, 43]^T ≡ [8,10]^T; adding e gives b ≡ [9,10]^T (mod 11). // Answer
Tags: Calculation Week8 LWE
<!--ID: 1764867990970-->
END

START
Basic
Regev encryption: Let p = 11. For embedding message bit m = 1 into c1, which value is added (before noise)? // Question
Back: (p − 1)/2 = (11 − 1)/2 = 5. // Answer
Tags: Calculation Week8 Regev
<!--ID: 1764867990973-->
END

START
Basic
Hybrid encryption: A KEM generates a 256-bit symmetric key, and the DEM encrypts a 1 KB (1024-byte) message. What is the input length to the symmetric cipher? // Question
Back: The symmetric cipher encrypts the 1 KB message → 1024 bytes. // Answer
Tags: Calculation Week13 KEMDEM
<!--ID: 1764867990976-->
END

START
Basic
HMAC: If the underlying hash function outputs 128-bit digests, what is the HMAC tag length? // Question
Back: The HMAC tag length is 128 bits (the hash output length). // Answer
Tags: Calculation Week13 HMAC
<!--ID: 1764867990978-->
END
