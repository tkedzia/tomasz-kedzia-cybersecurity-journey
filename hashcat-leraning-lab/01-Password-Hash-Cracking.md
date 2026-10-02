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
- After cracking the hash I could logged in to juice-shop with exploited credentials:admin@juice-sh.op admin123

- Hashcat attempts to discover passwords by:

Generating password candidates.
Hashing them.
Comparing generated hashes with target hashes.
Reporting matches. [Cyber Secu...edzia) (4) | Word]
Showing already-cracked passwords

After a cracking session:

--show

displays recovered passwords.

Important:

--show does NOT crack hashes.
It only displays previously recovered results. [Cyber Secu...edzia) (4) | Word]
Multiple hashes

Hashcat can:

process many hashes simultaneously
solve them in parallel
use GPU acceleration where available. [Cyber Secu...edzia) (4) | Word]
Backend devices

Purpose:

Select GPU/CPU devices used for cracking.
Discovery workflow
Detect available compute devices.
Review device IDs.
Choose relevant device(s).
Use backend device parameter if needed.

If omitted:

Hashcat usually chooses automatically. [Cyber Secu...edzia) (4) | Word]
Password attack types
Brute Force

Tries combinations systematically.

Example idea:

a
b
c
...
aa
ab
ac
...

Very expensive for long passwords. [Cyber Secu...edzia) (4) | Word]

Dictionary Attack

Uses:

common passwords
word lists
known password databases

Hashes those candidates and compares results. [Cyber Secu...edzia) (4) | Word]

Rainbow Table Attack

Uses precomputed password→hash mappings.

Difference from dictionary attack:

Dictionary attack computes hashes during execution.
Rainbow tables use previously computed hashes. [Cyber Secu...edzia) (4) | Word]
