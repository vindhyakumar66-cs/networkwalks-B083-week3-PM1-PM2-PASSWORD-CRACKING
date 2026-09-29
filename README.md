# 🔓 Week 3 Cybersecurity Internship: PDF Password Cracking & Hash Analysis

This repository documents the work completed for **Week 3 (Project Module 1&2)** of my Cybersecurity Internship with Networkwalks — cracking the password of a locked PDF file using both a browser-based workflow and John the Ripper (via the Johnny GUI).

The goal of the lab is to understand how password hashes are extracted from protected files and how weak passwords can be recovered using dictionary attacks.

---

## 🛠️ Lab Overview & Scope

- **Module:** Week 3 – Project Module 1: Password Cracking with John the Ripper
                       Project Module 2: Password Cracking with Networkwalks Tools
- **Instructor:** Waqas Karim
- **Provider:** Networkwalks Academy
- **Target File:** `My Locked PDF1.pdf` (password-protected PDF supplied with the task)
- **Environment:** Windows laptop (browser-based tools) + Kali Linux (John the Ripper / Johnny GUI)

---

## 🚀 Lab Objectives & Workflow

1. **Hash Extraction** – Pull the `$pdf$` crackable hash out of the locked PDF.
2. **Browser-Based Cracking** – Use the Networkwalks **Hash Calculator** and **Password Cracker** web tools to recover the password without installing anything.
3. **Offline Cracking** – Extract the same hash with `pdf2john` and crack it locally using **John the Ripper**, driven through the **Johnny** GUI, as a second independent method.
4. **Verification** – Open the PDF with the recovered password to confirm the crack was successful.

---

## 🔑 Method 1: Networkwalks Web Tools

### Step 1 – Extract the hash
- Opened the [Networkwalks Hash Calculator](https://networkwalks.com/hash-calculator/) in the browser.
- Uploaded `My Locked PDF1.pdf` under the **PDF** tab.
- The tool parsed the file locally (nothing uploaded to a server) and returned a hashcat/John-compatible hash starting with `$pdf$...`.


### Step 2 – Crack the hash
- Copied the full `$pdf$` hash.
- Opened the [Networkwalks Password Cracker](https://networkwalks.com/password-cracker/).
- Pasted the hash and ran the attack using the built-in wordlist.
- The tool tried each candidate password until it found a match.

<img width="1600" height="934" alt="HASH CALCULATER PDF1" src="https://github.com/user-attachments/assets/c3a344e4-faa2-44a7-9dd4-0215ae59b496" />

### Result

| Tool | Hash Source | Wordlist | Cracked Password |
|---|---|---|---|
| Networkwalks Password Cracker | Networkwalks Hash Calculator | Built-in list (100 words) | `password1` |
<img width="1600" height="870" alt="PASSWORD FOUND" src="https://github.com/user-attachments/assets/88a0d52a-c830-40d1-a973-1fe765080561" />

### Step 3 – Verify
- Opened `My Locked PDF1.pdf` and entered `password1` as the document password.
- File opened successfully.
<img width="1062" height="936" alt="PDF UNLOCKED" src="https://github.com/user-attachments/assets/a719f52b-68b0-468c-937a-be0e44ffbaa3" />

---

## 🔑 Method 2: John the Ripper via Johnny GUI (Kali Linux)

### Step 1 – Extract the hash with `pdf2john`
```bash
pdf2john "My Locked PDF1.pdf" > hash1.txt


```
ALTERNATE METHOD EXTRACT FROM www.onlinehashcrack.com
<img width="1484" height="927" alt="ONLINE HASH CRACK" src="https://github.com/user-attachments/assets/1a9b6230-35d1-4e73-8008-04d81eaa0509" />
### Step 2 – Load the hash into Johnny
- Opened **Johnny** (the GUI front-end for John the Ripper).
- Loaded `hash1.txt` as the password hash file.
- Selected the `rockyou.txt` wordlist (decompressed first if needed):
```bash
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
```
- Started the attack from within the Johnny interface.
<img width="1600" height="827" alt="JONHNNY GUI" src="https://github.com/user-attachments/assets/c4e792b4-dfad-4b12-b01e-2e65af9fcede" />

### Step 3 – Equivalent CLI command
The same attack from the terminal, for reference:
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt
```
To view a cracked password once John finishes:
```bash
john --show hash1.txt
```

### Result

| Tool | Hash Source | Wordlist | Cracked Password |
|---|---|---|---|
| John the Ripper (Johnny GUI) | `pdf2john` | `rockyou.txt` | `password1` |
<img width="1062" height="938" alt="PDF2 UNLOCKED" src="https://github.com/user-attachments/assets/b61b06dd-ee02-4b98-9bdf-e65ecebd6f86" />


### Step 4 – Verify
- Opened `My Locked PDF1.pdf` with the password shown by Johnny/John — confirmed it matched the result from the web tool.
<img width="1062" height="936" alt="PDF UNLOCKED" src="https://github.com/user-attachments/assets/4e655a64-8edc-44b6-880f-d6e29c1aa455" />

---

## 📊 Comparison of Methods

| Aspect | Networkwalks Web Tools | John the Ripper (Johnny GUI) |
|---|---|---|
| Installation | None — runs in browser | Requires Kali Linux / JtR install |
| Hash extraction | Built-in (drag & drop PDF) | `pdf2john` command |
| Wordlist | Small built-in list (100 words) | Large `rockyou.txt` (14M+ passwords) |
| Speed | Fast for weak/common passwords | Depends on wordlist size and hardware |
| Best for | Quick checks, no setup | Real-world audits, larger/complex passwords |

Both methods converged on the same cracked password (`password1`), confirming the result.

---

## 📌 Key Security Takeaways

- **Password complexity matters** – a common word like `password1` is cracked almost instantly against any dictionary, web-based or offline.
- **Hashing vs. encryption** – the PDF's password is stored as a one-way hash, not encrypted plaintext, so cracking means guessing until a hash matches, not "decrypting" it.
- **Wordlist size matters** – the offline `rockyou.txt` attack covers far more real-world passwords than a small built-in list, making it useful for weaker or less-common passwords.
- **Best practice** – use long, unique passphrases mixing uppercase, lowercase, numbers, and symbols to make dictionary and brute-force attacks impractical.

---

## 📂 Repository Contents

- `README.md` – this write-up
- Screenshots documenting each step (hash extraction, cracking in progress, cracked password, PDF unlock confirmation) for both methods

---

## 🙏 Acknowledgments

Special thanks to instructor **Waqas Karim** and **Networkwalks** for the hands-on lab environment and guided task.

**Disclaimer:** All activities in this repository were carried out strictly for educational purposes on files provided for this authorized lab exercise.
