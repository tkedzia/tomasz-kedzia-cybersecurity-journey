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
   cd C:\hashcat-7.1.2
   What this does
    cd = Change Directory
    C:\hashcat-7.1.2 = Folder where Hashcat is installed

   .\hashcat.exe
   .\ = Run executable from current folder
    hashcat.exe = Hashcat program
2. Identify available compute devices
    .\hashcat.exe -I
    -I = Information
3. Create a file containing the hash
   "0192023a7bbd73250516f069df18b500" | Out-File hashes.txt
4. Identify the hash type
   (Get-Content hashes.txt).Length
   32 = MD5 (Hashcat mode 0)
    40 = SHA1 (Hashcat mode 100)
    64 = SHA256 (Hashcat mode 1400)
    Why we're doing this
    Hashcat needs to know which hashing algorithm was used. The value passed with the -m option in later commands depends on the hash type.
5. Run Hashcat to identify available compute devices
    Before starting the attack, check which CPU/GPU devices Hashcat can use.  
## hashcat Components Used

## Observations

## Challenges

## Lessons Learned

## Next Steps



