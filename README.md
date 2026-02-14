# Subtron - Advanced Subdomain Enumeration Toolkit

![banner](https://img.shields.io/badge/Subdomain%20Enum-Toolkit-green?style=for-the-badge)
![status](https://img.shields.io/badge/Status-Stable-blue?style=for-the-badge)

---

## 🔍 About
`Subtron` is an **automated subdomain enumeration script** written in **Bash**, built for bug hunters, penetration testers, and security researchers.  

It combines multiple powerful tools:
- [Amass](https://github.com/owasp-amass/amass)
- [Subfinder](https://github.com/projectdiscovery/subfinder)
- [Assetfinder](https://github.com/tomnomnom/assetfinder)
- [Httpx](https://github.com/projectdiscovery/httpx)
- `crt.sh` (certificate transparency logs)

All results are deduplicated and stored neatly in `results/<domain>/`.

---

## ⚡ Features
- Runs **multiple enumeration tools in parallel**  
- **Silent mode** → only shows status messages, not noisy tool output  
- Saves **unique domains** & **live hosts**  
- Colorized output with progress messages  
- Organized results per domain  

---

## 📦 Installation

Clone the repository and run the setup script:

```bash
git clone https://github.com/atulxerma/subtron.git
cd subtron
chmod +x **
./setup
```
# This installs:

golang

amass

subfinder

assetfinder

httpx
jq, curl

---

## 🚀 Usage
```
subtron example.com
```
## Example Output
```
[+] Target Domain: example.com
[+] Output Folder: results/example.com
[=] Scan Finished!
[+] Unique domains saved to: results/example.com/domains.txt
[+] Live hosts saved to: results/example.com/live_hosts.txt
Total Domains Found: 342
Live Hosts: 127
```
## 📂 Results

After running, you’ll find:
```
results/
└── example.com/
    ├── amass_passive.txt
    ├── amass_brute.txt
    ├── subfinder.txt
    ├── assetfinder.txt
    ├── crtsh.txt
    ├── domains.txt       # merged + deduplicated subdomains
    └── live_hosts.txt    # live hosts from httpx
```
## ⚠️ Disclaimer

This tool is for educational and authorized security testing only.
The author is not responsible for misuse or illegal activities.

## ✨ Author

Created by @cyb3ratul

