# 🤖🔍 AI-Powered Recon: Using Gemini + Nmap for Automated Security Scans

After reading the article:
https://hackers-arise.com/artificial-intelligence-in-cybersecurity-using-ai-for-port-scanning/ by @Hackers-arise which guides on how to set up a CLI LLM to conduct nmap scans using “LLM NMAP”:https://github.com/peter-hackertarget/llm-tools-nmap

I decided to give it a try, as someone who uses “nmap” for most networking and security activities I spend most of the time in the man pages for syntaxes or articles on the different scripts for Nmap Scripting Engine (NSE) for my different needs this process makes the job more simple, efficient and productive.

![cloning tool from github](images/001.png)

## 🖥️ Setup & Tools Used:
To set it up I used my ubuntu server VM 🖥️, Installed and configured “llm” python package which gives us the power to use a CLI based LLM. It was configured so that it would use “gemini-2.5-flash” with the api key from my account. Then with the cloned “LLM NMAP” from GitHub, the LLM was able to generate Nmap commands and execute them through the system, allowing automation of scanning tasks.

So to simply say the tools/resources used were:

-🔧 Pipx — python based isolation installs

-🤖 Llm — python CLI interface for LLM

-🔑 Gemini api — use gemini with “llm”

-🧩 llm-tools-nmap — llm to command nmap

-📡 nMap


![installing llm package](images/002.png)

![setting up gemini with llm package](images/003.png)

![4th image](images/005.png)


## 🔎 Tests Performed

🔹 Basic Active Host Scan

Used the following prompt:

```bash
llm --functions llm-tools-nmap.py "scan this network 192.168.10.0/24 and tell me the hosts that are up with their IP"
```



The output was detailed, well organized, and easy to understand even though it was a CLI interface.

![5th image](images/005.png)

```bash
llm --functions llm-tools-nmap.py "Gather as much information as you can and identify any security issues or items of interest to a security analyst 192.168.10.148"
/root/.local/pipx/venvs/llm/lib/python3.10/site-packages/pydantic/main.py:1804: UserWarning: Field name "register" in "NoneInputSchema" shadows an attribute in parent "BaseModel"
  return meta(
The following information was gathered for 192.168.10.148:

**Open Ports and Services:**

*   **22/tcp:** SSH (OpenSSH 8.9p1 Ubuntu 3ubuntu0.13, protocol 2.0)
*   **80/tcp:** HTTP (nginx 1.18.0 Ubuntu) - Runs Jellyfin, and `/` is a disallowed entry in `robots.txt`.
*   **139/tcp:** NetBIOS Session Service (Samba smbd 4.6.2)
*   **445/tcp:** Microsoft-DS (Samba smbd 4.6.2)
*   **8096/tcp:** Unknown service - likely Jellyfin as it's a common port for it.

**Operating System:**

*   Linux (Ubuntu, based on OpenSSH and Nginx versions).
*   Nmap's OS detection suggests `cpe:/o:linux:linux_kernel`.

**Samba Information:**

*   Samba `smbd 4.6.2` is running.
*   SMB2 security mode: Message signing is enabled but not required.

**Items of Interest to a Security Analyst:**

1.  **Outdated Samba Version:** Samba `4.6.2` is quite old. This version likely has known vulnerabilities. Upgrading Samba to a newer, patched version is highly recommended.
2.  **Jellyfin Exposed on Port 80:** Jellyfin is a media server. Exposing it directly on port 80 without HTTPS means all traffic, including login credentials, is sent in plaintext. This is a significant security risk. It should be configured to use HTTPS.
3.  **SSH Open:** While necessary for remote administration, OpenSSH should be secured. Recommendations include:
    *   Disabling root login.
    *   Using strong, complex passwords or, preferably, SSH keys for authentication.
    *   Considering a firewall to limit SSH access to trusted IPs.
    *   Keeping OpenSSH updated. The current version `8.9p1` is relatively recent, but regular patching is still important.
4.  **SMB Message Signing Not Required:** Although enabled, the fact that SMB message signing is not *required* could potentially allow for man-in-the-middle attacks where an attacker could tamper with SMB traffic. It's generally a good practice to require SMB signing.
5.  **Unknown Service on Port 8096:** While likely Jellyfin, any unknown open port warrants further investigation to understand its purpose and assess potential risks.
6.  **`robots.txt` Disallowed Entry:** The `/` entry in `http-robots.txt` for the Nginx server indicates that the server administrator doesn't want search engines to index the root directory. While not a security vulnerability in itself, it shows an attempt at controlling access/visibility and might hint at sensitive content.

**Recommendations:**

*   **Upgrade Samba:** Update Samba to the latest stable version to mitigate known vulnerabilities.
*   **Enable HTTPS for Jellyfin:** Configure Nginx and Jellyfin to use HTTPS to encrypt all web traffic. Obtain and install a valid SSL/TLS certificate.
*   **Secure SSH:** Implement best practices for SSH security (disable root login, use SSH keys, restrict access).
*   **Require SMB Signing:** Configure Samba to *require* SMB message signing.
*   **Investigate Port 8096:** Confirm the service running on port 8096 and secure it appropriately. If it is Jellyfin, ensure it's also behind HTTPS or not directly exposed if not intended.
*   **Regular Patching:** Ensure all software on the system (OS, Nginx, OpenSSH, Samba, Jellyfin) is kept up-to-date with the latest security patches.
*   **Firewall:** Implement a host-based firewall (e.g., UFW on Ubuntu) to restrict access to only necessary ports and trusted IP addresses.
```
Detailed output from prompt

## ✅ Overall Pros for me:
⚡ AI-generated command accuracy
⚡ How fast it built the scan sequence
📄 Human-readable summaries
🛡️ CVE insights / weak service identification
🧠 “Security analyst”-style output

## ⚠️ Cons / Issues Faced:
Personally no cons for me was so great and appreciate it so much. But its not perfect — supervision and thorough inspection and verification will be needed at times.

🔐 The one and only issue I faced which can be solved is allowing certain permissions to make certain tasks run.
