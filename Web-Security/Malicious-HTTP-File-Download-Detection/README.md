# Malicious HTTP File Download Detection

## Overview

This challenge focuses on analyzing HTTP traffic within a packet capture (PCAP) to identify a suspicious executable downloaded by a workstation.

## Challenge Information

* **Platform:** PKCERT CTF Square
* **Category:** Web Security
* **Difficulty:** Easy
* **Points:** 100

## Objective

Identify the name of the malicious file downloaded over HTTP by analyzing the provided PCAP.

## Tools Used

* Wireshark

## Investigation Steps

1. Opened the provided PCAP file in Wireshark.
2. Applied the `http` display filter.
3. Reviewed HTTP GET requests.
4. Identified the request for a suspicious executable.
5. Confirmed the downloaded filename from the HTTP traffic.
   
<img width="1711" height="716" alt="Malicious HTTP File Download Detection" src="https://github.com/user-attachments/assets/68bc6e98-5253-42db-8058-67ba6c847ec5" />

## Key Evidence

* HTTP GET request for:

  ```
  /trojan_update.exe
  ```

* HTTP response indicated an executable download (`application/x-msdos-program`).

## Result

Successfully identified the downloaded malicious file:

```
trojan_update.exe
```

## Skills Practiced

* Network traffic analysis
* HTTP protocol analysis
* PCAP investigation
* Identifying downloaded files in network captures

## Key Takeaways

* Begin with protocol filtering to reduce noise.
* HTTP GET requests frequently expose downloaded filenames.
* Focus on the exact requirement stated in the challenge before performing additional analysis.

