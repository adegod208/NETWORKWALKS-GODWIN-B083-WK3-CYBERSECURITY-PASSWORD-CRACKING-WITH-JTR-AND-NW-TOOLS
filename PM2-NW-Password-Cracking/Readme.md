# PM2 – Cracking PDF Passwords with Networkwalks' Hash Calculator & Password Cracker
 
## Overview
This lab demonstrates an end-to-end, browser-only workflow for recovering the password on a locked PDF. Instead of installing a local tool like John the Ripper, everything is done through two free web utilities from Networkwalks:
 
- **Hash Calculator** – pulls a `pdf2john` / hashcat-style hash out of an encrypted PDF, processed locally in the browser.
- **Password Cracker** – runs a dictionary attack against that hash using a wordlist of your choice.
## Goals
- Pull a crackable hash out of a password-protected PDF using the Hash Calculator.
- Run that hash through the Password Cracker's dictionary attack.
- See firsthand how much the size and quality of a wordlist affects whether a crack succeeds.
- Recover the PDF's password and grab the module flag.
## Tools & Environment
 
| Tool | Purpose |
|---|---|
| [Networkwalks Hash Calculator](https://networkwalks.com/hash-calculator/) | Extracts a hash from a PDF locally in-browser — nothing is uploaded |
| [Networkwalks Password Cracker](https://networkwalks.com/password-cracker/) | Runs a dictionary attack against a pasted hash |
| OS | Windows |
 
**Target files:** `My Locked PDF1.pdf`, `My Locked PDF2.pdf`
 
## Walkthrough
 
### Part A — Cracking `My Locked PDF2.pdf`
 
**1. Pull the hash**
Loaded `My Locked PDF2.pdf` into the Hash Calculator's PDF tab. Since parsing happens entirely client-side, the file never left the browser. The tool returned an encrypted hash in `pdf2john`/hashcat format (Revision R4, Version V4, 128-bit key).
 ![](path-or-url-to-image)
**2. Attack the hash**
Copied the hash into the Password Cracker and kicked off a dictionary attack using the tool's built-in 100-word list. Under the hood this works the same way John the Ripper does: hash each candidate word and compare it to the target hash.
 
**3. Success**
The stock 100-word list was enough — the correct password turned up on the 91st attempt.
 
**4. Flag captured**
Used the recovered password to open the PDF and grabbed the flag for this module.
 
### Part B — Revisiting `My Locked PDF1.pdf`
 
**Problem: the default wordlist wasn't big enough**
Reusing the hash already extracted for `PDF1` back in PM1, I ran the same 100-word built-in list against it. All 100 candidates were tried with no match — access denied.
 
This wasn't a broken hash or a broken tool; the wordlist just didn't happen to contain the real password. The cracker even suggested the next move itself: try a bigger list.
 
**Fix: swap in a much larger wordlist**
Uploaded `JTR_default_password.txt`, a list of 3,500+ candidate passwords, and reran the attack against the same hash.
 
**Result**
The correct password was found near the end of the run, at roughly 97% progress (3,456 of 3,556 words tried): `good-luck`.
 
This matched the password recovered for the identical hash back in PM1 using John the Ripper / Johnny — a nice confirmation that two independent tools agree on the answer.
 
## Key Takeaways
- Networkwalks' Hash Calculator can generate a `pdf2john`-compatible hash entirely client-side — no need to upload the target file anywhere.
- A dictionary attack is only as good as the list behind it. A failed attempt doesn't mean the method or tool is broken — it just means the right word wasn't in the list.
- Scaling the wordlist from 100 entries to 3,500+ turned a hard "access denied" into a successful crack.
- Validating a cracked password against two different tools (John the Ripper in PM1, the Networkwalks web cracker here) is a solid way to build confidence in the result.
- For real-world password audits, wordlist quality and coverage (think `rockyou.txt`-scale lists) matter more than which cracking tool you use.
 

