TARGET DECK: Cryptography

START
Basic
This is a perfectly secure real-or-random symmetric encryption scheme if and only if it uses a One-Time Pad. // Question
Back: True. OTP is the only perfectly secure real-or-random encryption scheme. // Answer
Tags: Week1 OTP PerfectSecrecy
<!--ID: 1764865836433-->
END

START
Basic
Encrypting a message with OTP using k1 and then again with OTP using k2 is equivalent to a single OTP with key k1 ⊕ k2. // Question
Back: True. OTP composes via XOR of keys. // Answer
Tags: Week1 OTP XOR
<!--ID: 1764865836434-->
END

START
Basic
OTP followed by AES encryption remains perfectly secure. // Question
Back: False. AES is only computationally secure, so perfect secrecy is lost. // Answer
Tags: Week1 OTP AES
<!--ID: 1764865836435-->
END

START
Basic
A Pseudorandom Permutation with block size B can be distinguished from a PRF with B-bit input/output using about 2^(B/2) samples. // Question
Back: True. This follows from the birthday bound. // Answer
Tags: Week3 PRP PRF Birthday
<!--ID: 1764865836437-->
END

START
Basic
PRPs are abstractions of public-key encryption. // Question
Back: False. PRPs are symmetric-key primitives. // Answer
Tags: Week3 PRP
<!--ID: 1764865836438-->
END

START
Basic
AES expands the key into round keys and applies repeated rounds to the state. // Question
Back: True. AES is an iterated substitution–permutation network. // Answer
Tags: Week4 AES
<!--ID: 1764865836439-->
END

START
Basic
AES follows the Feistel construction. // Question
Back: False. AES is not Feistel-based. // Answer
Tags: Week4 AES Feistel
<!--ID: 1764865836440-->
END

START
Basic
ECB mode is IND-CPA secure when used with AES. // Question
Back: False. ECB is deterministic and leaks block equality. // Answer
Tags: Week4 ECB INDCPA
<!--ID: 1764865836441-->
END

START
Basic
CTR mode encryption is c = m ⊕ E_k(nonce || counter). // Question
Back: True. CTR turns the block cipher into a stream cipher. // Answer
Tags: Week4 CTR
<!--ID: 1764865836442-->
END

START
Basic
Reusing a nonce in CTR mode is as dangerous as reusing a key in OTP. // Question
Back: True. It reveals m ⊕ m'. // Answer
Tags: Week4 CTR Nonce
<!--ID: 1764865836443-->
END

START
Basic
Encrypt-then-MAC is generically IND-CCA secure. // Question
Back: True. It provides confidentiality and integrity. // Answer
Tags: Week5 EtM INDCCA
<!--ID: 1764865836444-->
END

START
Basic
MAC-then-Encrypt is generically IND-CCA secure. // Question
Back: False. It enables padding oracle attacks. // Answer
Tags: Week5 MtE
<!--ID: 1764865836445-->
END

START
Basic
A MAC provides confidentiality. // Question
Back: False. MACs provide integrity and authenticity only. // Answer
Tags: Week5 MAC
<!--ID: 1764865836446-->
END

START
Basic
EUF-CMA security means an attacker cannot produce a valid tag for a new message. // Question
Back: True. That is the definition of EUF-CMA. // Answer
Tags: Week5 EUFCMA
<!--ID: 1764865836447-->
END

START
Basic
CBC-MAC is secure for variable-length messages without modification. // Question
Back: False. It is only secure for fixed-length messages. // Answer
Tags: Week5 CBCMAC
<!--ID: 1764865836448-->
END

START
Basic
A hash function is collision resistant if it is hard to find x ≠ x' such that H(x)=H(x'). // Question
Back: True. That is the definition of collision resistance. // Answer
Tags: Week6 Hash Collision
<!--ID: 1764865836449-->
END

START
Basic
Finding a collision in a 256-bit hash requires about 2^256 operations. // Question
Back: False. It requires about 2^128 operations due to the birthday bound. // Answer
Tags: Week6 Birthday
<!--ID: 1764865836450-->
END

START
Basic
Salting passwords prevents brute-force attacks entirely. // Question
Back: False. Salting only prevents precomputation and rainbow tables. // Answer
Tags: Week6 Salting
<!--ID: 1764865836451-->
END

START
Basic
Merkle–Damgård hashes are vulnerable to length-extension attacks. // Question
Back: True. Attackers can append data without knowing the message. // Answer
Tags: Week6 MerkleDamgard
<!--ID: 1764865836452-->
END

START
Basic
H(k || m) is a secure MAC construction. // Question
Back: False. It is vulnerable to length-extension. // Answer
Tags: Week13 HMAC
<!--ID: 1764865836453-->
END

START
Basic
HMAC prevents length-extension attacks. // Question
Back: True. HMAC wraps the hash in a secure PRF-like construction. // Answer
Tags: Week13 HMAC
<!--ID: 1764865836454-->
END

START
Basic
Textbook RSA is IND-CPA secure. // Question
Back: False. It is deterministic and malleable. // Answer
Tags: Week7 RSA
<!--ID: 1764865836455-->
END

START
Basic
RSA encryption is c = m^e mod N. // Question
Back: True. That is textbook RSA encryption. // Answer
Tags: Week7 RSA
<!--ID: 1764865836456-->
END

START
Basic
RSA-OAEP randomizes RSA to achieve IND-CCA security in the ROM. // Question
Back: True. OAEP adds randomness and non-malleability. // Answer
Tags: Week7 OAEP
<!--ID: 1764865836457-->
END

START
Basic
ElGamal encryption is IND-CCA secure. // Question
Back: False. It is IND-CPA secure but malleable. // Answer
Tags: Week8 ElGamal
<!--ID: 1764865836458-->
END

START
Basic
The Discrete Log Problem asks for x given g and g^x. // Question
Back: True. That is the DLP. // Answer
Tags: Week8 DLP
<!--ID: 1764865836459-->
END

START
Basic
DDH hardness implies CDH hardness. // Question
Back: True. DDH is the stronger assumption. // Answer
Tags: Week8 DDH CDH
<!--ID: 1764865836460-->
END

START
Basic
Learning With Errors remains hard even for quantum computers. // Question
Back: True. This is why LWE is post-quantum secure. // Answer
Tags: Week8 LWE PQC
<!--ID: 1764865836461-->
END

START
Basic
Regev’s PKE is based on the hardness of LWE. // Question
Back: True. Its security reduces to Decision-LWE. // Answer
Tags: Week8 Regev
<!--ID: 1764865836462-->
END

START
Basic
A digital signature provides non-repudiation if the public key is identity-bound. // Question
Back: True. Unforgeability enables non-repudiation. // Answer
Tags: Week9 Signatures
<!--ID: 1764865836463-->
END

START
Basic
Replaying a valid signed message is a cryptographic forgery. // Question
Back: False. It is a protocol-level replay attack, not a forgery. // Answer
Tags: Week9 Replay
<!--ID: 1764865836464-->
END

START
Basic
Textbook RSA signatures are secure under EUF-CMA. // Question
Back: False. They are trivially forgeable. // Answer
Tags: Week9 RSA Signatures
<!--ID: 1764865836465-->
END

START
Basic
RSA-FDH signs H(m)^d mod N. // Question
Back: True. Full-Domain Hashing signs the hash. // Answer
Tags: Week9 RSAFDH
<!--ID: 1764865836466-->
END

START
Basic
Elliptic curves provide the same security as RSA with much smaller key sizes. // Question
Back: True. ECC is more efficient per bit of security. // Answer
Tags: Week10 ECC
<!--ID: 1764865836467-->
END

START
Basic
Invalid-curve attacks exploit missing validation of received curve points. // Question
Back: True. They can leak private-key information. // Answer
Tags: Week10 ECC Attacks
<!--ID: 1764865836468-->
END

START
Basic
Forward secrecy protects past messages if a long-term key is later compromised. // Question
Back: True. That is exactly the FS definition. // Answer
Tags: Week11 FS
<!--ID: 1764865836469-->
END

START
Basic
Post-compromise security protects messages sent before compromise. // Question
Back: False. It protects messages after recovery. // Answer
Tags: Week11 PCS
<!--ID: 1764865836470-->
END

START
Basic
X3DH uses multiple Diffie–Hellman computations to establish an initial shared secret. // Question
Back: True. It combines identity, signed, ephemeral, and one-time keys. // Answer
Tags: Week11 X3DH
<!--ID: 1764865836471-->
END

START
Basic
The Signal Double Ratchet provides both Forward Secrecy and Post-Compromise Security. // Question
Back: True. It combines symmetric and DH ratchets. // Answer
Tags: Week11 DoubleRatchet
<!--ID: 1764865836472-->
END

START
Basic
Shor’s algorithm efficiently factors integers and solves discrete logs. // Question
Back: True. This breaks RSA, DH, and ECC. // Answer
Tags: Week12 Shor
<!--ID: 1764865836473-->
END

START
Basic
Grover’s algorithm completely breaks AES-256. // Question
Back: False. It only gives a quadratic speedup. // Answer
Tags: Week12 Grover
<!--ID: 1764865836474-->
END

START
Basic
AES-256 provides about 128-bit security against quantum attackers. // Question
Back: True. Grover reduces security by half. // Answer
Tags: Week12 AES Quantum
<!--ID: 1764865836475-->
END

START
Basic
Mosca’s theorem states that if x+y>z then migration must start immediately. // Question
Back: True. It formalizes quantum migration urgency. // Answer
Tags: Week12 Mosca
<!--ID: 1764865836476-->
END

START
Basic
Post-quantum cryptography uses classical algorithms secure against quantum attacks. // Question
Back: True. It does not require quantum hardware. // Answer
Tags: Week12 PQC
<!--ID: 1764865836477-->
END

START
Basic
KEM–DEM splits public-key encryption into key transport and data encryption. // Question
Back: True. This is the hybrid encryption paradigm. // Answer
Tags: Week13 KEMDEM
<!--ID: 1764865836478-->
END

START
Basic
Encrypt-and-MAC is generically IND-CCA secure. // Question
Back: False. Ciphertext integrity is not guaranteed. // Answer
Tags: Week13 Composition
<!--ID: 1764865836479-->
END

START
Basic
A KDF turns biased or structured secrets into uniform cryptographic keys. // Question
Back: True. That is the main purpose of KDFs. // Answer
Tags: Week13 KDF
<!--ID: 1764865836480-->
END

START
Basic
Side-channel attacks break cryptographic schemes by violating hardness assumptions. // Question
Back: False. They bypass the model via physical leakage. // Answer
Tags: Week13 SideChannels
<!--ID: 1764865836481-->
END

START
Basic
Poor randomness can completely destroy cryptographic security. // Question
Back: True. Many real-world breaks are due to RNG failures. // Answer
Tags: Week13 Randomness
<!--ID: 1764865836482-->
END

START
Basic
A real-or-random perfectly secure symmetric key encryption scheme can only be constructed using the One-Time Pad. // Question
Back: False. Counterexample: encrypt with OTP and then append a 0 bit; decryption ignores the last bit. Still perfectly secure. // Answer
Tags: Week1 OTP PerfectSecrecy
<!--ID: 1764866216992-->
END

START
Basic
Encrypting with OTP using k1 and then again with OTP using k2 is equivalent to a single OTP with key k1 ⊕ k2. // Question
Back: True. OTP composes by XORing the keys. // Answer
Tags: Week1 OTP XOR
<!--ID: 1764866217000-->
END

START
Basic
Encrypting with OTP and then encrypting with AES-128 destroys perfect secrecy. // Question
Back: False. Any operation independent of the message and OTP key preserves perfect secrecy. // Answer
Tags: Week1 OTP AES
<!--ID: 1764866217005-->
END

START
Basic
If P is a PRP, then P(k2, P(k1, x)) is a PRP with key k1||k2. // Question
Back: True. Composition of PRPs yields a PRP with extended key length. // Answer
Tags: Week3 PRP Composition
<!--ID: 1764866217009-->
END

START
Basic
A PRP with block-size B can be distinguished from a PRF with B-bit input/output using about B/2 samples. // Question
Back: False. About 2^{B/2} queries are required by the birthday bound. // Answer
Tags: Week3 PRP PRF Birthday
<!--ID: 1764866217014-->
END

START
Basic
Pseudorandom Permutations are an abstraction of public-key encryption. // Question
Back: False. PRPs abstract block ciphers (symmetric crypto). // Answer
Tags: Week3 PRP
<!--ID: 1764866217018-->
END

START
Basic
AES expands the key into round keys and applies repeated rounds using those keys. // Question
Back: True. AES is an iterated substitution–permutation network. // Answer
Tags: Week4 AES
<!--ID: 1764866217023-->
END

START
Basic
AES follows the Feistel design pattern. // Question
Back: False. AES is an SPN, not a Feistel cipher. // Answer
Tags: Week4 AES Feistel
<!--ID: 1764866217027-->
END

START
Basic
IND-CPA security of CTR mode fails if the random value r is reused. // Question
Back: True. Reuse creates a two-time pad attack. // Answer
Tags: Week4 CTR Nonce
<!--ID: 1764866217032-->
END

START
Basic
ECB applies a secure PRF to each block and is IND-CPA secure. // Question
Back: False. ECB uses a PRP and is deterministic and insecure. // Answer
Tags: Week4 ECB
<!--ID: 1764866217036-->
END

START
Basic
Any PRF with output length λ can be used as a EUF-CMA secure MAC. // Question
Back: True. PRF output can serve directly as a MAC tag. // Answer
Tags: Week5 MAC PRF
<!--ID: 1764866217041-->
END

START
Basic
The CBC-MAC tag length scales with the message length. // Question
Back: False. It equals the block length of the block cipher. // Answer
Tags: Week5 CBCMAC
<!--ID: 1764866217046-->
END

START
Basic
A MAC protects both confidentiality and integrity. // Question
Back: False. MACs protect integrity only. // Answer
Tags: Week5 MAC
<!--ID: 1764866217051-->
END

START
Basic
A cryptographic hash function maps variable-length input to fixed-length output deterministically. // Question
Back: True. That is the definition. // Answer
Tags: Week6 Hash Definition
<!--ID: 1764866217055-->
END

START
Basic
OAEP can use a cryptographic hash function to make RSA IND-CCA secure. // Question
Back: True. OAEP randomizes RSA using hashes. // Answer
Tags: Week7 OAEP
<!--ID: 1764866217060-->
END

START
Basic
Merkle–Damgård construction prevents length-extension attacks. // Question
Back: False. Merkle–Damgård is vulnerable to length-extension. // Answer
Tags: Week6 MerkleDamgard
<!--ID: 1764866217066-->
END

START
Basic
The RSA public exponent e is often chosen as 4. // Question
Back: False. 4 divides φ(N); e must be coprime to φ(N). // Answer
Tags: Week7 RSA
<!--ID: 1764866217070-->
END

START
Basic
If φ(N) can be computed efficiently, RSA is insecure. // Question
Back: True. φ(N) allows computation of the private key. // Answer
Tags: Week7 RSA
<!--ID: 1764866217075-->
END

START
Basic
ElGamal can only be constructed securely when discrete logs are hard. // Question
Back: False. DDH hardness is needed; DLP hardness alone is insufficient. // Answer
Tags: Week8 ElGamal
<!--ID: 1764866217080-->
END

START
Basic
ElGamal public-key encryption is IND-CPA secure. // Question
Back: True. It is secure under DDH. // Answer
Tags: Week8 ElGamal
<!--ID: 1764866217084-->
END

START
Basic
Digital signature schemes use a shared secret key. // Question
Back: False. They use public/private key pairs. // Answer
Tags: Week9 Signatures
<!--ID: 1764866217089-->
END

START
Basic
If CDH is easy, then DDH is also easy in the same group. // Question
Back: True. CDH hardness implies DDH hardness. // Answer
Tags: Week8 CDH DDH
<!--ID: 1764866217094-->
END

START
Basic
X3DH is quantum secure. // Question
Back: False. It relies on quantum-broken Diffie–Hellman. // Answer
Tags: Week11 X3DH Quantum
<!--ID: 1764866217099-->
END

START
Basic
Shor’s algorithm breaks RSA using period finding. // Question
Back: True. It factors N in polynomial time. // Answer
Tags: Week12 Shor
<!--ID: 1764866217103-->
END

START
Basic
Quantum computers break all practical cryptography in polynomial time. // Question
Back: False. Symmetric crypto is only quadratically weakened. // Answer
Tags: Week12 Quantum
<!--ID: 1764866217108-->
END

START
Basic
There is a known polynomial-time quantum algorithm for solving LWE. // Question
Back: False. LWE remains hard even for quantum attackers. // Answer
Tags: Week8 LWE
<!--ID: 1764866217113-->
END

START
Basic
In Regev encryption, the message is used as the secret vector s. // Question
Back: False. The message is added as a large error term. // Answer
Tags: Week8 Regev
<!--ID: 1764866217117-->
END

START
Basic
Kerckhoffs’ principle says the key should be secret but the algorithm public. // Question
Back: True. Security must rely only on the secrecy of the key. // Answer
Tags: Week1 Kerckhoffs
<!--ID: 1764866217121-->
END

START
Basic
IND-CPA security allows chosen-ciphertext attacks. // Question
Back: False. That is IND-CCA security. // Answer
Tags: Week2 INDCPA INDCCA
<!--ID: 1764866217126-->
END

START
Basic
A properly generated OTP ciphertext is uniformly random. // Question
Back: True. That is why it is perfectly secure. // Answer
Tags: Week1 OTP
<!--ID: 1764866217131-->
END

START
Basic
ECB mode is recommended for encrypting long messages. // Question
Back: False. ECB leaks block patterns. // Answer
Tags: Week4 ECB
<!--ID: 1764866217137-->
END

START
Basic
Counter mode outputs k+1 blocks of ciphertext for k plaintext blocks. // Question
Back: True. One block is used for the nonce/IV. // Answer
Tags: Week4 CTR
<!--ID: 1764866217142-->
END

START
Basic
CBC-MAC is secure against length-extension attacks. // Question
Back: False. It is vulnerable without fixes like ECBC. // Answer
Tags: Week5 CBCMAC
<!--ID: 1764866217148-->
END

START
Basic
A PRF with sufficient output length can be used directly as a MAC. // Question
Back: True. PRF outputs serve as MAC tags. // Answer
Tags: Week5 MAC PRF
<!--ID: 1764866217153-->
END

START
Basic
Hash functions are always keyed in practice. // Question
Back: False. Hash functions are typically unkeyed. // Answer
Tags: Week6 Hash
<!--ID: 1764866217160-->
END

START
Basic
A 128-bit hash should collide after about 2^64 queries. // Question
Back: True. This follows from the birthday bound. // Answer
Tags: Week6 Birthday
<!--ID: 1764866217165-->
END

START
Basic
RSA arithmetic is modulo a prime. // Question
Back: False. It is modulo the product of two primes. // Answer
Tags: Week7 RSA
<!--ID: 1764866217171-->
END

START
Basic
If factoring is easy, RSA is broken. // Question
Back: True. Factoring gives φ(N) and thus d. // Answer
Tags: Week7 RSA
<!--ID: 1764866217177-->
END

START
Basic
The private RSA exponent must be random and unpredictable. // Question
Back: True. It is the secret key. // Answer
Tags: Week7 RSA
<!--ID: 1764866217233-->
END

START
Basic
Plain RSA encryption is IND-CPA secure. // Question
Back: False. It is deterministic. // Answer
Tags: Week7 RSA
<!--ID: 1764866217240-->
END

START
Basic
Plain RSA signatures use the RSA decryption algorithm. // Question
Back: True. Signing computes m^d mod N. // Answer
Tags: Week9 RSA Signatures
<!--ID: 1764866217245-->
END

START
Basic
Diffie–Hellman requires three protocol messages. // Question
Back: False. It uses two messages. // Answer
Tags: Week10 DH
<!--ID: 1764866217249-->
END

START
Basic
Diffie–Hellman can be done in one simultaneous round. // Question
Back: True. Both parties can send g^a and g^b at the same time. // Answer
Tags: Week10 DH
<!--ID: 1764866217253-->
END

START
Basic
Diffie–Hellman is secure against active MITM attacks. // Question
Back: False. Authentication is required. // Answer
Tags: Week10 DH MITM
<!--ID: 1764866217257-->
END

START
Basic
Diffie–Hellman outputs equal keys when no active attacker is present. // Question
Back: True. Both compute g^{ab}. // Answer
Tags: Week10 DH
<!--ID: 1764866217261-->
END

START
Basic
Quantum computers completely break block ciphers. // Question
Back: False. They only weaken them quadratically via Grover. // Answer
Tags: Week12 Quantum AES
<!--ID: 1764866217265-->
END

START
Basic
Elliptic-curve Diffie–Hellman is quantum resistant. // Question
Back: False. Shor’s algorithm breaks ECDH. // Answer
Tags: Week12 Quantum ECC
<!--ID: 1764866217272-->
END

START
Basic
The LWE problem is believed to be quantum secure. // Question
Back: True. It underlies post-quantum cryptography. // Answer
Tags: Week8 LWE Quantum
<!--ID: 1764866217274-->
END

START
Basic
In Regev encryption, the message is encoded as a large error term. // Question
Back: True. It shifts the ciphertext for correct decryption. // Answer
Tags: Week8 Regev
<!--ID: 1764866217278-->
END
