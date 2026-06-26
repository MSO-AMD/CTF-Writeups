# The Forgotten Metadata – Digital Forensics CTF Write-up

**Platform:** PKCERT CTF  
**Category:** Digital Forensics / Log Forensics  
**Difficulty:** Easy

---

## A Note for Beginners

If you're new to Digital Forensics or CTFs, this challenge is a great reminder that not every flag is hidden behind advanced steganography or complex tools.

Sometimes, simply reading the challenge description carefully and inspecting the available metadata is enough.

One thing I've learned so far is that every challenge adds another pattern to your toolkit. The more patterns you recognize, the faster you'll solve similar challenges in the future.

---

## Challenge Overview

The challenge provided a JPG image and stated that investigators believed the original owner had left behind hidden information.

The description specifically mentioned inspecting the image metadata, so I decided to begin there instead of trying multiple steganography tools.

---

# Initial Approach

Since I had recently completed another metadata-based challenge, I reused the same workflow.

I uploaded the image to:

**https://exif.tools/**

and carefully reviewed every available metadata field.

---

## Metadata Analysis

While inspecting the metadata, I noticed a Windows-specific EXIF field named **XPKeywords**.

Instead of normal keywords, it contained the following value:

```text
UEtDRVJUe21ldGFkYXRhX2RldGVjdGl2ZX0=
```

The string immediately stood out because:

- It only contained valid Base64 characters.
- It ended with the `=` padding character.
- It matched the format of a Base64-encoded string.

<img width="1612" height="472" alt="exiftool" src="https://github.com/user-attachments/assets/abedb72a-699c-49ad-af4e-251252db896b" />

---

## Decoding the Data

I copied the encoded string into **CyberChef**.

Recipe used:

```
From Base64
```

The decoded output revealed the complete flag.

<img width="1677" height="585" alt="cyberchef-frombase64" src="https://github.com/user-attachments/assets/6fb1f009-5221-4696-a8d8-6478fa7a22b7" />

---

## Lesson Learned

This challenge introduced me to **Windows XP metadata fields**, which can sometimes contain hidden information.

Some useful metadata fields worth checking during Digital Forensics challenges include:

- User Comment
- Image Description
- XPComment
- XPKeywords
- XPTitle
- XPSubject

Even though these fields are intended for storing image information, CTF authors often use them to hide encoded messages.

---

## Tools Used

- Exif.tools
- CyberChef

---

## Key Takeaways

This challenge reinforced three important habits:

- Always inspect every available metadata field.
- Learn to recognize common encodings such as Base64.
- Follow the challenge description carefully—the hints often point directly toward the correct technique.

---

## Final Thoughts

Although this was an easier challenge, it demonstrated how valuable pattern recognition becomes over time.

A previous challenge taught me to inspect image metadata for hidden information.

This time, I immediately recognized a suspicious Base64 string inside **XPKeywords**, decoded it with CyberChef, and recovered the flag within minutes.

Every completed CTF adds another pattern to my Digital Forensics toolkit, making future investigations more intuitive.

---

## Protocol Zero Journey

This write-up is part of **Protocol Zero**—my long-term cybersecurity learning journey where every project, lab, CTF, and lesson is documented to track progress and help other beginners learn through practical experience.

### CTF Progress

| # | Challenge | Category | Difficulty | Status |
|---|-----------|----------|------------|--------|
| 1 | Scattered Secrets | Digital Forensics | Hard | Completed |
| 2 | The Forgotten Metadata | Digital Forensics | Easy | Completed |

> **One Project. One Challenge. One Lesson. At a Time.**
