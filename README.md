# 📖README — Blueprint (TryHackMe)
## TL;DR (the part you actually care about)
Found an exposed `install.php` on osCommerce → RCE → got a PowerShell reverse shell → grabbed `SAM` & `SYSTEM` → dumped NTLM hashes → cracked creds. Walked out richer by one flag.
## Quick steps
* Scan: `nmap -p- --min-rate 2000 -T4 <IP>` then targeted `-sC -sV` on the juicy ports.
* Found `install.php` on port 80/8080 → ran public osCommerce RCE (yes it’s still a thing).
* Spawned an encoded PS reverse shell (UTF-16LE base64) → `nc -nvlp <port>` to catch it.
* Saved hives: `reg save HKLM\SAM SAM` & `reg save HKLM\SYSTEM SYSTEM`.
* Pulled the hives off the box (tiny PS HTTP server + `certutil.exe`/`wget`).
* `secretsdump.py -sam SAM -system SYSTEM LOCAL` → profit. Crack with `hashcat` or whatever lazy online site you prefer.

## Pro tips (from someone who’s been lazy and effective UwU )

* Always check for leftover installer pages — easiest wins.
* `certutil` = Windows’ weird little Swiss Army knife. Use it. Don’t be scared.
* Don’t be a knob: only test boxes you have permission for.

## Final flex

Got root, grabbed flags, took screenshots, left no breadcrumbs (well… mostly).
