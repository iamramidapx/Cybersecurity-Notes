# Cryptography: Study Notes

A concise summary covering cryptography fundamentals, public-key cryptography, and hashing concepts.

## Table of Contents

1. [Cryptography Basics](#1-cryptography-basics)
2. [Public-Key Cryptography](#2-public-key-cryptography)
3. [Hashing](#3-hashing)
4. [Quick Reference](#4-quick-reference)

---

## 1. Cryptography Basics

Cryptography secures communication in the presence of adversaries, protecting **confidentiality**, **integrity**, and **authenticity**. It shows up everywhere: HTTPS logins, SSH sessions, bank-certificate checks, and file-integrity verification. Standards such as PCI DSS (payment cards) and HIPAA / GDPR (medical data) require encrypting data both **at rest** and **in transit**.

### Key terms

| Term | Meaning |
|------|---------|
| Plaintext | Original readable data |
| Ciphertext | Scrambled output of encryption |
| Cipher | Algorithm that converts plaintext to ciphertext and back |
| Key | Bits used by the cipher; the cipher is public, the key is secret |
| Encryption / Decryption | Plaintext → ciphertext / ciphertext → plaintext |

### Historical ciphers

- **Caesar cipher**: shift each letter by a fixed amount. Only 25 possible keys, so it is trivially broken today.
- Others of note: Vigenère (16th c.), Enigma (WWII), one-time pad (Cold War).

### Symmetric encryption

One shared secret key encrypts and decrypts. Fast, but the key must be distributed over a secure channel.

- **DES**: 56-bit key, considered broken since the late 1990s; obsolete.
- **3DES**: DES applied three times (~112-bit effective security); deprecated in 2019.
- **AES**: current standard since 2001; 128/192/256-bit keys.

### Asymmetric encryption

A key **pair**: encrypt with the public key, decrypt with the private key. Slower and uses larger keys, relying on math problems that are easy forward and infeasible to reverse.

- **RSA**: 2048-bit minimum recommended.
- **Diffie-Hellman**: key agreement; 2048-bit minimum.
- **ECC**: equivalent security with shorter keys (256-bit ECC ≈ 3072-bit RSA).

### Math building blocks

- **XOR (⊕)**: outputs 1 when bits differ. Key properties: `A ⊕ A = 0`, `A ⊕ 0 = A`, commutative, associative. So `C = P ⊕ K` and `C ⊕ K = P` — a simple symmetric cipher (only secure if the key is as long as the message and never reused).
- **Modulo (%)**: the remainder after division. It is not reversible (many inputs map to one output), which is part of what makes it useful in cryptography.

---

## 2. Public-Key Cryptography

### Common pattern

Use asymmetric crypto once to exchange a symmetric key, then switch to fast symmetric encryption for the rest of the session. Real systems also add digital signatures and certificates to verify identity.

### RSA

Security rests on the difficulty of **factoring the product of two large primes**: multiplying is easy, factoring is not. Public key = `(n, e)`, private key = `(n, d)`, where `n = p × q` for large primes `p, q`. Encryption and decryption use modular exponentiation with these values.

### Diffie-Hellman key exchange

Lets two parties derive a shared secret over an insecure channel without ever transmitting the secret itself:

1. Agree publicly on a prime `p` and generator `g`.
2. Each side picks a private number and derives a public value from it.
3. They exchange public values and each combines the other's public value with their own private number to arrive at the same shared secret.

DH alone doesn't authenticate either party, so it's typically paired with RSA/ECDSA signatures to guard against man-in-the-middle attacks.

### SSH

- **Server authentication**: the client checks the host key fingerprint (shown on first connection) and stores it in `known_hosts`.
- **Client authentication**: password or key pair. Key pairs are generally more secure.
- Private keys should be protected with a passphrase and strict file permissions; the passphrase never leaves the local machine.
- Public keys trusted by a server are listed in `~/.ssh/authorized_keys`.

### Digital signatures and certificates

- A **digital signature** is typically a hash of a document encrypted with the signer's private key; anyone can verify it with the public key, proving authenticity and integrity.
- **TLS certificates** bind a public key to a domain. Trust flows through a chain from the site's certificate up to a root **Certificate Authority** pre-trusted by the OS/browser. Let's Encrypt issues free certificates.
- **PGP / GPG** provides file encryption and signing: share your public key, keep the private key secret and backed up.

### Terminology

- **Cryptanalysis**: the study of breaking cryptosystems without the key.
- **Brute force**: trying every possible key.
- **Dictionary attack**: trying likely candidate words.

---

## 3. Hashing

A hash function maps input of any size to a fixed-size digest. There is **no key**, and a secure hash is infeasible to reverse. A one-bit change in the input should change the output completely.

### Properties and uses

- Fast to compute, impractical to invert, designed to resist collisions.
- Main uses: **file integrity** checks and **password verification**.
- Output is raw bytes, usually displayed as hex or base64.

### Collisions

Two different inputs producing the same hash. Because inputs are unlimited and outputs are finite, collisions must exist in principle (pigeonhole principle); a good hash function makes *finding* one computationally infeasible.

- **MD5** and **SHA-1** both have known practical collision weaknesses and are no longer considered secure for integrity or security purposes.
- SHA-256 or stronger is the modern recommendation.

### Password storage: good vs. bad practice

| Approach | Verdict | Why |
|----------|---------|-----|
| Plaintext | Insecure | A single breach exposes every password directly |
| Reversible/weak encryption | Insecure | If the key leaks, every password is recoverable |
| Fast, unsalted hash (e.g. SHA-1) | Insecure | Vulnerable to precomputed lookup ("rainbow") tables |
| Salted, slow password-hashing function (Argon2, scrypt, bcrypt, PBKDF2) | Recommended | Unique salts defeat lookup tables; deliberate slowness resists brute force |

- A **salt** is a unique, non-secret random value stored alongside each user's hash. It ensures identical passwords produce different hashes and defeats precomputed rainbow tables.
- Password managers are a different case: they must recover the original password, so they rely on encryption rather than hashing.

### Operating-system password storage (for background/awareness)

- **Linux** stores password hashes in `/etc/shadow` (root-readable only), in the form `$prefix$options$salt$hash`, where the prefix identifies the algorithm (e.g. `$y$` for yescrypt, the modern default; `$2b$`/`$2y$` for bcrypt; `$6$` for sha512crypt; `$1$` for the now-weak md5crypt).
- **Windows** stores hashed credentials (NTLM) in the SAM database, or in `NTDS.dit` on domain controllers.

---
