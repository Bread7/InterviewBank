---
description: >-
  A brief exploration of the impact quantum computing will have on the world of
  cryptography as it becomes mainstream.
---

# Quantum Computing: Unveiling the Cryptographic Paradigm Shift

**What does Quantum Computing entail?**

Quantum computing is an upcoming approach that utilizes the principles of quantum mechanics, like superposition and entanglement, to perform calculations fundamentally impossible for classical computers. Instead of traditional bits (0 or 1), quantum computers use qubits that can exist in multiple states simultaneously. This allows for massively parallel computations, opening the door to solving problems in areas like drug discovery, material design, cryptography, and artificial intelligence. While still in its early stages, quantum computing has the potential to transform industries and reshape our technological landscape.

{% embed url="https://www.ibm.com/topics/quantum-computing" %}

Threat to Cryptographic Algorithms

The advent of quantum computing presents a significant threat to current cryptographic algorithms, which are foundational to the security of digital communications and data. Most existing cryptographic systems rely on the computational difficulty of certain mathematical problems as the basis for their security. However, quantum computers can solve these problems much more efficiently than classical computers.

**Vulnerable Algorithms (Insert Applied Crypto PTSD here)**

1. **RSA**: RSA depends on the factorization of large prime numbers, a task that is extremely time-consuming for classical computers. Quantum computers, however, could efficiently factor these numbers using Shor's Algorithm, effectively breaking RSA encryption.
2. **Elliptic Curve Cryptography (ECC)**: ECC is another widely used encryption technique based on the elliptic curve discrete logarithm problem. Like RSA, ECC's security is compromised by quantum computers because of their ability to quickly solve discrete logarithms with Shor’s Algorithm.
3. **Diffie-Hellman Key Exchange**: Used for securely exchanging cryptographic keys over a public channel, the Diffie-Hellman exchange is also vulnerable to quantum computing for the same reasons as ECC.

#### Implications for Data Security

The ability of quantum computers to break these cryptographic systems has profound implications:

* **Privacy Breach**: The decryption of previously secure communications.
* **Data Integrity Threats**: The potential for data manipulation becomes more feasible if cryptographic checksums can be bypassed.
* **Economic and National Security Risks**: Significant risks to financial systems and national defense infrastructures that rely on cryptography for security.

Thankfully, [NIST](https://csrc.nist.gov/Projects/Post-Quantum-Cryptography) has been working on standardizing new, quantum-safe algorithms that will address asymmetric encryption and key agreement, as well as digital signatures. As a quick overview, the algorithms look as follows, for our chosen security level.

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption><p><a href="https://bughunters.google.com/blog/5108747984306176/google-s-threat-model-for-post-quantum-cryptography">https://bughunters.google.com/blog/5108747984306176/google-s-threat-model-for-post-quantum-cryptography</a><br></p></figcaption></figure>

#### Quantum-Resistant Cryptographic Methods

In response to these vulnerabilities, researchers and cryptographers are developing quantum-resistant cryptographic methods, often referred to as post-quantum cryptography (PQC). These methods are designed to be secure against both quantum and classical computers.

**Prominent Quantum-Resistant Algorithms**

1. **Lattice-based Cryptography**: This approach involves constructing cryptographic primitives based on lattice problems, which are believed to be hard for quantum computers to solve. Lattice-based methods include schemes like NTRU and algorithms for fully homomorphic encryption, which allows computation on encrypted data.
2. **Hash-based Cryptography**: This method uses secure hash functions to construct signatures. Hash-based signatures, such as the Lamport signatures, are simple and believed to be secure against quantum attacks.
3. **Code-based Cryptography**: Relying on error-correcting codes, code-based cryptography remains secure because decoding arbitrary linear codes is a problem not efficiently solvable by quantum computers. The McEliece cryptosystem is a notable example.
4. **Multivariate Quadratic Equations**: Cryptography based on systems of multivariate quadratic equations is another promising area of PQC. This technique involves solving equations that are computationally difficult for both classical and quantum computers.

#### Conclusion

The rise of quantum computing heralds a transformative era in computational capabilities, presenting significant challenges to the field of cryptography. The current cryptographic algorithms that secure everything from internet transactions to government communications could be rendered obsolete. Which is why it is imperative to address these challenges. The development of quantum-resistant cryptography is a critical step in maintaining the security and privacy standards in the quantum age. As quantum computing technology continues to advance, the need for widespread adoption and implementation of post-quantum cryptography will become increasingly urgent, ensuring the safeguarding of our digital and real-world assets.



**Interview Questions**\
\
**What is quantum computing, and why is it considered a significant threat to current cryptographic systems?**&#x20;

**Can you explain the differences between symmetric and asymmetric cryptography in the context of quantum resistance?**&#x20;

**Can you describe at least two quantum-resistant cryptographic algorithms and explain how they provide security against quantum attacks.**&#x20;

**How should organizations prepare for the transition to quantum-resistant cryptography? What steps would you recommend for an organization to start implementing today?**



Here are some articles pertaining to this topic (Cheers Guys! :D)

{% embed url="https://www.quantum.gov/nist-announces-first-four-quantum-resistant-cryptographic-algorithms/" %}

{% embed url="https://arxiv.org/abs/2112.00399" %}

## Author

* [Nikhil](https://github.com/KR0N0S99)
