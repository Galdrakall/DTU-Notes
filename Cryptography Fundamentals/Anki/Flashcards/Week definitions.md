TARGET DECK: Cryptography-Definitions

START
Basic
What is a Symmetric-Key Encryption Scheme? // Question
Back: A triple of algorithms (KeyGen, Enc, Dec) where KeyGen generates a secret key k, Enc(k,m) encrypts a message m, and Dec(k,c) decrypts a ciphertext c such that Dec(k,Enc(k,m)) = m. // Answer
Tags: Week1 Definition SymmetricEncryption
<!--ID: 1764866426144-->
END

START
Basic
What is Perfect Secrecy? // Question
Back: An encryption scheme is perfectly secret if for all messages m0,m1 and ciphertexts c, Pr[Enc(k,m0)=c] = Pr[Enc(k,m1)=c]. // Answer
Tags: Week1 Definition PerfectSecrecy
<!--ID: 1764866426158-->
END

START
Basic
What is the One-Time Pad (OTP)? // Question
Back: A perfectly secret encryption scheme where ciphertext c = m ⊕ k with a uniformly random key k of the same length as m. // Answer
Tags: Week1 Definition OTP
<!--ID: 1764866426163-->
END

START
Basic
What is IND-CPA security? // Question
Back: A scheme is IND-CPA secure if no efficient adversary can distinguish encryptions of two chosen messages with non-negligible advantage. // Answer
Tags: Week2 Definition INDCPA
<!--ID: 1764866426168-->
END

START
Basic
What is a Negligible Function? // Question
Back: A function f(n) is negligible if for every polynomial p(n), f(n) < 1/p(n) for all sufficiently large n. // Answer
Tags: Week3 Definition Negligible
<!--ID: 1764866426173-->
END

START
Basic
What is a Pseudorandom Function (PRF)? // Question
Back: A family of efficiently computable functions that is computationally indistinguishable from a truly random function. // Answer
Tags: Week3 Definition PRF
<!--ID: 1764866426177-->
END

START
Basic
What is a Pseudorandom Permutation (PRP)? // Question
Back: A family of keyed permutations that is computationally indistinguishable from a uniformly random permutation. // Answer
Tags: Week3 Definition PRP
<!--ID: 1764866426182-->
END

START
Basic
What is the Birthday Bound? // Question
Back: The principle that collisions in a random function with n-bit outputs occur with high probability after about 2^{n/2} samples. // Answer
Tags: Week3 Definition Birthday
<!--ID: 1764866426187-->
END

START
Basic
What is a Block Cipher? // Question
Back: A deterministic PRP with fixed input and output block size, keyed by a secret key. // Answer
Tags: Week4 Definition BlockCipher
<!--ID: 1764866426191-->
END

START
Basic
What is Electronic Codebook (ECB) Mode? // Question
Back: A mode that encrypts each block independently using the same block cipher key. // Answer
Tags: Week4 Definition ECB
<!--ID: 1764866426196-->
END

START
Basic
What is Cipher Block Chaining (CBC) Mode? // Question
Back: A mode where each plaintext block is XORed with the previous ciphertext block before encryption. // Answer
Tags: Week4 Definition CBC
<!--ID: 1764866426201-->
END

START
Basic
What is Counter (CTR) Mode? // Question
Back: A mode that turns a block cipher into a stream cipher by encrypting a nonce and counter sequence. // Answer
Tags: Week4 Definition CTR
<!--ID: 1764866426206-->
END

START
Basic
What is a Message Authentication Code (MAC)? // Question
Back: A keyed algorithm that produces a tag authenticating the integrity and origin of a message. // Answer
Tags: Week5 Definition MAC
<!--ID: 1764866426210-->
END

START
Basic
What is EUF-CMA Security? // Question
Back: Existential Unforgeability under Chosen Message Attack means no efficient attacker can forge a valid tag for a new message. // Answer
Tags: Week5 Definition EUFCMA
<!--ID: 1764866426215-->
END

START
Basic
What is Encrypt-then-MAC? // Question
Back: A composition where a message is first encrypted, then a MAC is computed over the ciphertext. // Answer
Tags: Week5 Definition EtM
<!--ID: 1764866426220-->
END

START
Basic
What is a Cryptographic Hash Function? // Question
Back: A deterministic function mapping variable-length input to fixed-length output with preimage, second-preimage, and collision resistance. // Answer
Tags: Week6 Definition Hash
<!--ID: 1764866426224-->
END

START
Basic
What is Preimage Resistance? // Question
Back: The property that given y it is computationally infeasible to find x such that H(x)=y. // Answer
Tags: Week6 Definition Preimage
<!--ID: 1764866426228-->
END

START
Basic
What is Second-Preimage Resistance? // Question
Back: The property that given x it is infeasible to find x'≠x such that H(x)=H(x'). // Answer
Tags: Week6 Definition SecondPreimage
<!--ID: 1764866426232-->
END

START
Basic
What is Collision Resistance? // Question
Back: The property that it is infeasible to find any x≠x' such that H(x)=H(x'). // Answer
Tags: Week6 Definition Collision
<!--ID: 1764866426237-->
END

START
Basic
What is the Merkle–Damgård Construction? // Question
Back: A method for building a hash function from a compression function by iterative chaining with padding. // Answer
Tags: Week6 Definition MerkleDamgard
<!--ID: 1764866426244-->
END

START
Basic
What is a Trapdoor One-Way Function? // Question
Back: A function that is easy to compute, hard to invert without secret information, and easy to invert with the trapdoor. // Answer
Tags: Week7 Definition Trapdoor
<!--ID: 1764866426249-->
END

START
Basic
What is RSA? // Question
Back: A public-key cryptosystem based on modular exponentiation where N=pq for large primes p and q. // Answer
Tags: Week7 Definition RSA
<!--ID: 1764866426255-->
END

START
Basic
What is RSA-OAEP? // Question
Back: A randomized padding transformation that makes RSA encryption IND-CCA secure in the Random Oracle Model. // Answer
Tags: Week7 Definition OAEP
<!--ID: 1764866426259-->
END

START
Basic
What is Hybrid Encryption? // Question
Back: A construction combining public-key encryption for key transport and symmetric encryption for data. // Answer
Tags: Week7 Definition Hybrid
<!--ID: 1764866426263-->
END

START
Basic
What is the Discrete Logarithm Problem (DLP)? // Question
Back: Given g and h=g^x in a group, find the exponent x. // Answer
Tags: Week8 Definition DLP
<!--ID: 1764866426269-->
END

START
Basic
What is Computational Diffie–Hellman (CDH)? // Question
Back: The problem of computing g^{ab} given g^a and g^b. // Answer
Tags: Week8 Definition CDH
<!--ID: 1764866426273-->
END

START
Basic
What is Decisional Diffie–Hellman (DDH)? // Question
Back: The problem of distinguishing (g^a,g^b,g^{ab}) from (g^a,g^b,g^c). // Answer
Tags: Week8 Definition DDH
<!--ID: 1764866426278-->
END

START
Basic
What is ElGamal Encryption? // Question
Back: A public-key encryption scheme based on exponentiation in a cyclic group and the DDH assumption. // Answer
Tags: Week8 Definition ElGamal
<!--ID: 1764866426282-->
END

START
Basic
What is the Learning With Errors (LWE) Problem? // Question
Back: Given (A,As+e), distinguish it from uniformly random data where e is small noise. // Answer
Tags: Week8 Definition LWE
<!--ID: 1764866426286-->
END

START
Basic
What is Regev’s Encryption Scheme? // Question
Back: A public-key encryption scheme whose IND-CPA security reduces to Decision-LWE. // Answer
Tags: Week8 Definition Regev
<!--ID: 1764866426291-->
END

START
Basic
What is a Digital Signature Scheme? // Question
Back: A triple of algorithms (KeyGen, Sign, Verify) enabling public verification of message authenticity. // Answer
Tags: Week9 Definition Signature
<!--ID: 1764866426295-->
END

START
Basic
What is EUF-CMA Security for Signatures? // Question
Back: The inability of an attacker to produce a valid signature on any new message even with signing oracle access. // Answer
Tags: Week9 Definition EUFCMASignature
<!--ID: 1764866426300-->
END

START
Basic
What is Forward Secrecy (FS)? // Question
Back: The property that past session keys remain secure even if long-term keys are compromised. // Answer
Tags: Week11 Definition FS
<!--ID: 1764866426305-->
END

START
Basic
What is Post-Compromise Security (PCS)? // Question
Back: The property that future session security recovers after an attacker loses access. // Answer
Tags: Week11 Definition PCS
<!--ID: 1764866426311-->
END

START
Basic
What is X3DH? // Question
Back: An authenticated key exchange protocol used to establish an initial shared secret for secure messaging. // Answer
Tags: Week11 Definition X3DH
<!--ID: 1764866426316-->
END

START
Basic
What is the Double Ratchet Algorithm? // Question
Back: A key update mechanism combining symmetric and Diffie–Hellman ratchets for FS and PCS. // Answer
Tags: Week11 Definition DoubleRatchet
<!--ID: 1764866426329-->
END

START
Basic
What is Shor’s Algorithm? // Question
Back: A quantum algorithm that factors integers and solves discrete logarithms in polynomial time. // Answer
Tags: Week12 Definition Shor
<!--ID: 1764866426336-->
END

START
Basic
What is Grover’s Algorithm? // Question
Back: A quantum search algorithm giving a quadratic speedup for brute-force attacks. // Answer
Tags: Week12 Definition Grover
<!--ID: 1764866426342-->
END

START
Basic
What is Post-Quantum Cryptography (PQC)? // Question
Back: Cryptography designed to remain secure against quantum computing attacks. // Answer
Tags: Week12 Definition PQC
<!--ID: 1764866426348-->
END

START
Basic
What is a Key Derivation Function (KDF)? // Question
Back: An algorithm that derives cryptographically strong keys from a shared secret or weak input. // Answer
Tags: Week13 Definition KDF
<!--ID: 1764866426353-->
END

START
Basic
What is HMAC? // Question
Back: A hash-based message authentication code that wraps a cryptographic hash to prevent length-extension attacks. // Answer
Tags: Week13 Definition HMAC
<!--ID: 1764866426359-->
END

START
Basic
What is a KEM–DEM Construction? // Question
Back: A hybrid encryption paradigm separating key encapsulation from data encryption. // Answer
Tags: Week13 Definition KEMDEM
<!--ID: 1764866426363-->
END

START
Basic
What is the Random Oracle Model (ROM)? // Question
Back: A proof model where hash functions are idealized as truly random functions. // Answer
Tags: Week13 Definition ROM
<!--ID: 1764866426369-->
END

START
Basic
What is Public-Key Infrastructure (PKI)? // Question
Back: A system of certificate authorities and trust chains for authenticating public keys. // Answer
Tags: Week13 Definition PKI
<!--ID: 1764866426375-->
END

START
Basic
What is a Side-Channel Attack? // Question
Back: An attack that exploits physical leakage such as timing, power, or memory access patterns. // Answer
Tags: Week13 Definition SideChannel
<!--ID: 1764866426381-->
END
