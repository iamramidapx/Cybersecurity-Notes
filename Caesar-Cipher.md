# Caesar Cipher: Cracking It Without the Key

> Part of the **CIA** (Confidentiality, Integrity, Availability) notes: Cryptography

## Core Idea

A Caesar cipher shifts every letter by the same fixed number **k** (the key).

- Only **26 possible keys** (0-25), so it is trivially breakable by **brute force**.
- One correct guess of a single word reveals the shift for the **whole message**.
- The mindset: you don't need to decode everything. **Find a pattern, guess one word, apply the shift to all.**

```
Encrypt: C = (P + k) mod 26
Decrypt: P = (C - k) mod 26
```

## Where Does the Shift Come From?

| Situation | What to do |
|-----------|------------|
| The problem gives the key | Use it directly, no guessing |
| No key given | Guess with patterns or brute force (0-25) |

**Common shifts to try first**

| Shift | Why |
|-------|-----|
| **3** | The classic; Julius Caesar's actual key |
| **13** | ROT13, very common in CTFs and labs |

## Cracking Strategy (fastest first)

1. **Read it by eye** and guess words.
2. **Try shift 3.**
3. **Try shift 13** (ROT13).
4. **Brute force** all shifts 0-25.
5. **Use a tool:** [dcode.fr/caesar-cipher](https://www.dcode.fr/caesar-cipher) or CyberChef.

### Pattern tricks

- **Short words:** 2-letter words are likely `IS`, `TO`, `AT`, `IN`, `OF`; 3-letter words are likely `THE`, `AND`, `YOU`.
- **Repeated-letter patterns:** letter repetition is preserved by the cipher. `WRPRUURZ` has the same pattern as `TOMORROW`.
- **Guess one word, derive the shift, apply everywhere.**

## Examples

### Example 1: Known pattern (shift 3)

```
DWWDFN WRPRUURZ  ->  ATTACK TOMORROW
```

`D -> A` is 3 back, so **key = 3**. Also `WR -> TO` confirms it.

### Example 2: ROT13

```
FVZCYR  ->  SIMPLE
```

Looks like broken English? Try 13 first.

### Example 3: Guess a short word (shift 11)

Ciphertext: `ESP DJDEPX TD LE CTDV`

1. Short words are `TD` and `LE`. Guess `TD = IS`.
2. `T -> I` and `D -> S` means shifting back **11**.
3. Apply to all words:

| Cipher | Plain |
|--------|-------|
| ESP | THE |
| DJDEPX | SYSTEM |
| TD | IS |
| LE | AT |
| CTDV | RISK |

**Plaintext: `THE SYSTEM IS AT RISK`, Key = 11**

Shortcut: guess `ESP = THE` first. It matches instantly (`E->T`, `S->H`, `P->E`).

### Example 4: Quick practice

| Ciphertext | Plaintext | Key |
|------------|-----------|-----|
| `KHOOR` | HELLO | 3 |
| `KHOOR ZRUOG` | HELLO WORLD | 3 |
| `XLMW` | THIS | 4 (shift back 4) |

## Code: Brute Force in Python

```python
def shift(text, k):
    out = []
    for ch in text.upper():
        if ch.isalpha():
            out.append(chr((ord(ch) - 65 - k) % 26 + 65))
        else:
            out.append(ch)
    return "".join(out)

cipher = "ESP DJDEPX TD LE CTDV"
for k in range(26):
    print(k, shift(cipher, k))   # k = 11 -> THE SYSTEM IS AT RISK
```

## Takeaways

- Caesar cipher is a **weak** cipher: tiny keyspace, and letter patterns and frequencies are preserved.
- Exam/lab shortcut: **eyeball -> try 3 -> try 13 -> brute force 0-25**.
- Guess one word, get the whole message.
