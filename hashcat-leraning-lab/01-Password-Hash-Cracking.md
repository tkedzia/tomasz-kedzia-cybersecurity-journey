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
- TO CRACK: .\hashcat.exe --hash-type 0 --attack-mode 3 --optimized-kernel-enable --backend-devices 1 --workload-profile 1 --show hashes.txt
- TO SHOW THE CRACKING RESULT:.\hashcat.exe --hash-type 0 --attack-mode 3 --optimized-kernel-enable --backend-devices 1 --workload-profile 1 --show hashes.txt
## hashcat Components Used

## Observations

## Challenges

## Lessons Learned

## Next Steps



