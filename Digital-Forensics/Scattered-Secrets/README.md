
# Scattered Secrets – Digital Forensics CTF Write-up

Category: Digital Forensics
Difficulty: Hard
Platform: PKCERT CTF

> Hello, I'm MSO AMD.
> 

## A Note for Beginners

Before we begin, a message for beginners and anyone feeling lost in cybersecurity:

If you're new to CTFs, don't think that everyone solving challenges is some elite hacker with years of experience.

Many of us start by following write-ups, copying techniques, and learning how different tools work. That's completely normal.

When I started, I often had no idea what a challenge was asking for. The goal wasn't to solve everything immediately—it was to learn one new thing from each challenge.

Don't wait until you feel "ready" to start CTFs.

Start from Day 1.

Read write-ups. Follow walkthroughs. Use hints. Get stuck. Try again.

Over time, you'll notice that techniques that once looked impossible start becoming familiar.

If you're confused, overwhelmed, or feel like you're making no progress, that's completely normal. Most of my learning moments started with confusion. Cybersecurity often feels like trying random things until suddenly a small clue makes everything click.

This challenge was one of those moments for me. What looked like three ordinary files turned into a lesson in metadata analysis, Unicode steganography, and log forensics.

Scattered Secrets was a digital forensics challenge that required recovering a flag hidden across three different files. The challenge hint stated that each file used a different technique:

- Metadata analysis
- Zero-width character steganography
- HTTP status code binary decoding

The final flag could only be reconstructed after extracting all three fragments.

---

## Initial Approach

After downloading the challenge files, I started by reading the challenge description and hints carefully. Since the challenge explicitly mentioned three different techniques, I decided to investigate each file separately instead of trying random tools.

This turned out to be the correct approach because each file contained only a portion of the final flag.

---

## Part 1 – EXIF Metadata Analysis

The first file appeared to contain hidden information within its metadata.

To inspect the metadata, I used:

https://exif.tools/

Click **Browse** and upload the provided file.

<img width="1647" height="520" alt="image" src="https://github.com/user-attachments/assets/9e3a8d35-4536-44ea-b189-10c1cbc88aab" />


After uploading the file and reviewing the available metadata fields, I discovered a value stored in the **User Comment** field.


<img width="1300" height="422" alt="user-comment" src="https://github.com/user-attachments/assets/f1f2007c-8e0e-430c-ac75-5c306ec60152" />



This revealed the first fragment of the flag:

```
PKCERT{3x1f_d4t4_m4tt3rs_
```

### Lesson Learned

Metadata often contains hidden information that is not visible when viewing the file normally. Whenever a challenge mentions metadata, tools such as ExifTool or online metadata viewers should be among the first things to try.

---

## Part 2 – Zero-Width Character Steganography

The second file appeared normal when opened.

However, the challenge hint referenced invisible characters, which suggested zero-width Unicode steganography.

To investigate this, I used:

https://stegzero.com/

I selected the decode option and pasted the contents of the file.

<img width="1064" height="621" alt="lio" src="https://github.com/user-attachments/assets/57ca9a29-912e-4ff3-94d6-3c15b97cce66" />


The tool successfully detected hidden zero-width characters and revealed the second fragment:

```
z3r0w1dth_ch4rs_
```

### Lesson Learned

Zero-width spaces, joiners, and non-joiners are invisible to the human eye but can be used to hide messages inside text. This is a common beginner-friendly steganography technique in CTFs.

---

## Part 3 – HTTP Status Code Decoding

The third file was the most challenging and took the longest to solve.

This part took me the longest because I was not familiar with this type of log-based encoding.

My first approach was to extract all HTTP status codes from the log and try different binary mappings involving 200, 404, and 500 responses.

I even created a small table to test multiple combinations and decoding strategies. Despite several attempts, the output remained unreadable.

At that point, I realized I was focusing more on decoding than on understanding the data itself.

After taking another look at the log, I noticed that one endpoint appeared repeatedly:

```
/api/v1/heartbeat
```

That repetition stood out.

### Failed Attempts and Lessons

At this stage, I knew the solution involved HTTP status codes, but I didn't yet know the correct mapping.

I experimented with several combinations involving:

- 200
- 404
- 500

I created a small table and tested multiple binary mappings. Every attempt produced unreadable output, which told me that I was either using the wrong status codes or looking at too much data.

This was an important lesson: when a decoding attempt produces garbage repeatedly, stop brute-forcing and return to analyzing the source data.

<img width="1057" height="142" alt="failed-attempts" src="https://github.com/user-attachments/assets/93b7020b-43ae-49e5-a563-26deb66554c2" />

After taking a closer look at the log entries, I noticed a repeating request:

```
/api/v1/heartbeat
```

This repetitive pattern suggested that the hidden data was likely encoded within those entries rather than throughout the entire log.

### Using CyberChef

I imported the log(log file) into CyberChef 

The following workflow was performed in CyberChef:

1. Imported the report file.
2. Applied the Filter recipe.
3. Kept only lines containing:
/api/v1/heartbeat
4. Extracted HTTP status codes using Regular Expression extraction.
5. Replaced:
    - 200 → 0
    - 404 → 1
6. Decoded the resulting binary stream using From Binary.
<img width="925" height="872" alt="cyberchef-import-accesslog-file" src="https://github.com/user-attachments/assets/6530c80e-526e-4b2a-9b47-a4f7632c5f39" />
Add recipe : Filter 
Add Regix(keeps only)  :   /api/v1/heartbeat  
<img width="1936" height="782" alt="cyberchef-Filter" src="https://github.com/user-attachments/assets/a2634621-739d-4977-8ac8-686aceae86c8" />

After filtering the heartbeat entries, I copied the output back into the CyberChef input panel and cleared the previous recipe.

Next, I used the **Regular Expression Extractor** recipe to extract the HTTP status codes.

This produced a clean list containing only:

- 200
- 404
<img width="1912" height="802" alt="cyberchef-Extract" src="https://github.com/user-attachments/assets/195e0987-f8e1-4289-926d-251ef619c004" />

Paste the output again in input field and add recipie Find and replace
replace 200-0
404-1

<img width="1082" height="857" alt="cyberchef-Find Replace" src="https://github.com/user-attachments/assets/56655223-1a73-4c79-90aa-49cbfc9ba25e" />

The output was now a binary stream, which I decoded using the **From Binary** recipe.

<img width="875" height="535" alt="cyberchef-frombinary" src="https://github.com/user-attachments/assets/bac35ced-43a6-4070-a3a7-c54f01fb02e9" />

The process was:

1. Extract status codes.
2. Replace:
    - 200 → 0
    - 404 → 1
3. Remove whitespace.
4. Convert the binary stream using From Binary.

This produced the final fragment:

```
st4tus_c0d3s}
```

### Lesson Learned

When working with logs, repeated patterns often indicate hidden data. Rather than analyzing every line equally, identifying unusual repetition can significantly reduce the search space.

---

## Final Flag

Combining all three fragments produced:

```
PKCERT{3x1f_d4t4_m4tt3rs_z3r0w1dth_ch4rs_st4tus_c0d3s}
```

---

## Key Takeaways

This challenge helped me practice three common digital forensics techniques:

1. Metadata analysis
2. Unicode steganography
3. Log analysis and binary decoding

More importantly, it reinforced an important lesson:

Always pay attention to the challenge hints. In this case, the challenge description essentially provided the roadmap for solving every stage.

## Tools Used

- Exif.tools
- StegZero
- CyberChef
- Spreadsheet (for testing mappings)

## —

When I started this challenge, I focused mostly on tools.

By the end, I realized the real skill was observation.

The hints already pointed toward the solution:

- Metadata
- Invisible characters
- HTTP status codes

The challenge wasn't about finding a secret tool. It was about recognizing patterns and applying the correct technique.

That's a lesson I'll carry into future CTFs and forensic investigations.

## Final Thoughts

This challenge reinforced something I keep learning throughout my cybersecurity journey:

You do not need to know everything before starting.

Many techniques that seem complicated at first—metadata analysis, steganography, log analysis, binary decoding—become much easier once you've seen them in practice.

If you're a beginner reading this, don't be afraid to use write-ups, hints, and walkthroughs. Learning from others is part of the process.

The important thing is to stay curious, keep experimenting, and solve one challenge at a time.

Every challenge teaches something new, and those small lessons eventually build real skills.
