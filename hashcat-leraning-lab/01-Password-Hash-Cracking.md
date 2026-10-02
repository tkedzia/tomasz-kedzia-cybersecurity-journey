# Lab 001 - Password Hash Cracking

## Objective

Try cracking hashes using hashcat (you will need to adjust the --backend-devices parameter, you can also create test md5 hashes online): hashcat --hash-type 0 --attack-mode 3 --optimized-kernel-enable --backend-devices 3 --workload-profile 1 --show hashes.txt

## Environment

Tools:
Windows PowerShell
hashcat

Date:
2026-09-29-30

## Steps Performed

1. Installed and launched Hashcat.
- cd C:\hashcat-7.1.2
- What this does: C:\hashcat-7.1.2 = Folder where Hashcat is installed
- .\hashcat.exe
- .\ = Run executable from current folder
- hashcat.exe = Hashcat program
3. Identify available compute devices
-.\hashcat.exe -I
- -I = Information
- - Why we're doing this: Hashcat needs to know which compute device to use for cracking. The value passed with the --backend-devices option in later command depends on the selected copute device/-s.
4. Create a file containing the hash
  - "0192023a7bbd73250516f069df18b500" | Out-File hashes.txt
  - OR copy-paste the hash/es into the txt.file in hashcat directory   
5. Identify the hash type
- (Get-Content hashes.txt).Length
- 32 = MD5 (Hashcat mode 0)
- 40 = SHA1 (Hashcat mode 100)
- 64 = SHA256 (Hashcat mode 1400)
- Why we're doing this: Hashcat needs to know which hashing algorithm was used. The value passed with the --hash-type option in later command depends on the hash type.
7. Start the Hashcat attack
- TO CRACK: .\hashcat.exe --hash-type 0 --attack-mode 3 --optimized-kernel-enable --backend-devices 1 --workload-profile 1
- TO SHOW THE CRACKING RESULT:.\hashcat.exe --hash-type 0 --attack-mode 3 --optimized-kernel-enable --backend-devices 1 --workload-profile 1 --show hashes.txt
- OUTPUT: 0192023a7bbd73250516f069df18b500:admin123
## Challenges / Lessons Learned
- If you have only 1 compute device on your machine hashcat will use it by deafault
- Cracking of some hashes may run forever
- Your machine may overheat during the cracking and shut down = not enough cooling for such a demanding task
- Hashcat is not showing cracking results by deafult, you need to add --show parameter

# Hashcat related OTHER Notes

## How Hashcat Works

Hashcat attempts to discover passwords by:

1. Generating password candidates
2. Hashing those candidates
3. Comparing generated hashes with target hashes
4. Reporting any matches found

---

## Showing Cracked Passwords

After a cracking session:

```bash
--show
```

Displays previously recovered passwords.

### Important

- `--show` does **not** crack hashes.
- It only displays passwords that were already recovered during previous runs.

---

## Working with Multiple Hashes

Hashcat can:

- Process multiple hashes simultaneously
- Attempt to crack them in parallel
- Use GPU acceleration when supported by the hardware

---

## Backend Devices

### Purpose

Backend devices determine which CPU/GPU resources Hashcat uses for cracking.

### Discovery Workflow

1. Detect available compute devices
2. Review device IDs
3. Choose the appropriate device(s)
4. Specify them using the backend device parameter if needed

### Note

If no device is specified, Hashcat usually selects available devices automatically.

---

# Password Attack Types

## 1. Brute Force Attack

Tries password combinations systematically.

Example sequence:

```text
a
b
c
...
aa
ab
ac
...
```

### Characteristics

- Guarantees coverage of the defined search space
- Becomes extremely time-consuming for long or complex passwords

---

## 2. Dictionary Attack

Uses predefined password candidates such as:

- Common passwords
- Wordlists
- Password leak databases

Workflow:

1. Take a candidate from the wordlist
2. Generate its hash
3. Compare it against the target hash
4. Repeat until a match is found or the list is exhausted

### Advantages

- Much faster than brute force
- Effective against weak or commonly used passwords

---

## 3. Rainbow Table Attack

Uses precomputed password-to-hash mappings.

### How It Works

Instead of generating hashes during the attack:

- Password/hash combinations are calculated beforehand
- The attacker looks up hashes in the precomputed table

### Difference from Dictionary Attack

| Dictionary Attack | Rainbow Table Attack |
|------------------|---------------------|
| Computes hashes during execution | Uses precomputed hashes |
| Requires more computation | Requires more storage |
| More flexible | Faster lookups when tables already exist |

---
- After cracking the hash I could logged in to juice-shop with exploited credentials:admin@juice-sh.op admin123

