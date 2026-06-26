# Onion64 – Cryptography Encoding CTF Write-up

**Platform:** PKCERT CTF  
**Category:** Cryptography / Encoding  
**Difficulty:** Easy  

---

## A Note for Beginners

One of the biggest mistakes beginners make is assuming that every encoded message can be decoded in a single step.

Many CTF challenges apply multiple layers of encoding, requiring you to inspect the output after each decoding step before deciding what to do next.

This challenge was a great example of that mindset.

---

## Challenge Overview

The challenge description stated:

> *"Layers upon layers… Can you peel the onion and recover the hidden flag?"*

The name **Onion64** immediately suggested that the data had been encoded multiple times, similar to peeling the layers of an onion.

Instead of trying random recipes, I decided to inspect the output after every decoding step.

---

# Initial Observation

The provided string contained:

- Uppercase letters
- Lowercase letters
- Numbers
- Ended with `=`

These characteristics strongly suggested **Base64** encoding.

<img width="1727" height="756" alt="Base64Decode" src="https://github.com/user-attachments/assets/99410ea2-f127-4c68-99b8-faa65ecd490a" />

---

## Layer 1 – Base64

I copied the encoded text into **CyberChef** and applied:

```
From Base64
```

The output still appeared to be another encoded string.

Although the second string no longer ended with `=`, it still consisted only of characters commonly found in Base64.

Instead of assuming the decoding was complete, I decided to test another Base64 layer.

---

## Layer 2 – Base64

Applying **From Base64** a second time produced a different result.

This time, the output resembled hexadecimal values.



---

## Layer 3 – Hexadecimal

The decoded output looked like pairs of hexadecimal characters separated by spaces.

I applied:

```
From Hex
```
<img width="1522" height="503" alt="HexDecoding" src="https://github.com/user-attachments/assets/4d01ed5a-366a-49b9-b33c-d660e1a9e813" />

This converted the hexadecimal values into readable text.

However, the result was still incorrect:

```text
}gn1d0cn3_f0_n01pm4hC_u0Y{TRECKP
```

The text was readable, but clearly reversed.

---

## Final Layer – Reverse

The reversed text immediately stood out because:

- `TRECKP` looked like `PKCERT`
- The braces were reversed
- The overall structure resembled a flag written backwards

I applied the **Reverse** recipe in CyberChef.

The output revealed the complete flag.

<img width="965" height="535" alt="flag" src="https://github.com/user-attachments/assets/d66c1b9e-1458-450c-bf09-56ccea25c282" />


---

## Decode Chain

```
Encoded Data
      ↓
Base64
      ↓
Base64
      ↓
Hex
      ↓
Reverse
      ↓
Flag
```

---

## Lesson Learned

This challenge reinforced an important habit:

**Always inspect the output after each decoding step.**

Instead of repeatedly applying random recipes, pause and ask:

- Does this still look encoded?
- What encoding does it resemble now?
- Is the output readable but arranged incorrectly?

Following this process makes solving layered encoding challenges much easier.

---

## Tools Used

- CyberChef

---

## Key Takeaways

- Base64 strings do not always end with `=`.
- One decoding step is not always enough.
- Observe the output before choosing the next decoding method.
- Pattern recognition is more valuable than memorizing recipes.

---

## Final Thoughts

This challenge introduced me to layered encoding techniques.

Rather than relying on CyberChef's automatic detection, I practiced identifying each layer manually by observing how the output changed after every step.

Although the challenge was straightforward, it reinforced an important investigative habit that will be useful in more advanced cryptography and digital forensics challenges.

---

## Protocol Zero Journey

This write-up is part of **Protocol Zero**—my long-term cybersecurity learning journey where every project, lab, CTF, and lesson is documented to track progress and help other beginners learn through practical experience.

### CTF Progress

| # | Challenge | Category | Difficulty | Status |
|---|-----------|----------|------------|--------|
| 1 | Scattered Secrets | Digital Forensics | Hard | ✅ |
| 2 | The Forgotten Metadata | Digital Forensics | Easy | ✅ |
| 3 | Onion64 | Cryptography | Easy | ✅ |

> **One Project. One Challenge. One Lesson. At a Time.**
