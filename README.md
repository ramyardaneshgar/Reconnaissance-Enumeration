# HackTheBox Writeup: Reconnaissance & Enumeration using `whois`, `curl`, `gobuster`, and ReconSpider

## Context and Methodology
In this assessment, I demonstrated my proficiency in reconnaissance and information-gathering techniques using tools like `whois`, `curl`, `gobuster`, and the web crawler **ReconSpider**. The goal was to identify key details about the target domain, **inlanefreight.htb**, by analyzing DNS records, HTTP headers, and performing directory enumeration. My approach was systematic:

1. **Preparation:**
   - Connected to the Pwnbox instance to ensure a controlled and secure testing environment.
   - Updated the `/etc/hosts` file to map `inlanefreight.htb` to its IP address (`94.237.50.239`).
   - Verified connectivity using `ping` and `curl`.

2. **Execution:**
   - Used tools like `whois`, `curl`, `gobuster`, and ReconSpider to gather information.
   - Followed a structured process to collect, analyze, and interpret the findings.

---

## Question 1: What is the IANA ID of the registrar of the inlanefreight.com domain?

### Tools and Command:
I used the `whois` tool to retrieve the domain registration details. By piping the output through `grep`, I filtered for the specific field **IANA ID**:
```bash
whois inlanefreight.com | grep IANA
Methodology:
The whois command queries public registration data for domains.
The grep command filtered for the term IANA, making it easy to locate the registrar’s ID.
Question 2: What HTTP server software is powering the inlanefreight.htb site on the target system?
Tools and Command:
To identify the HTTP server software, I issued a HEAD request using the curl command:

bash
Copy code
curl -I http://inlanefreight.htb:40391
Methodology:
The curl tool allowed me to fetch HTTP headers without downloading the entire page content.
The -I flag ensured that only headers were retrieved, including the Server header, which often indicates the web server software.
Question 3: What is the API key in the hidden admin directory discovered on the target system?
Tools and Command:
I used gobuster to brute-force directories on the inlanefreight.htb domain. The command was:

bash
Copy code
gobuster dir -u http://inlanefreight.htb:40391 -w /usr/share/seclists/Discovery/Web-Content/common.txt
Methodology:
The gobuster tool scanned for directories and files by iterating through a wordlist (common.txt) to find hidden paths.
After discovering the /admin_h1dd3n directory, I used curl to examine its contents:
bash
Copy code
curl http://inlanefreight.htb:40391/admin_h1dd3n/
Question 4: After crawling the inlanefreight.htb domain on the target system, what is the email address you found?
Tools and Command:
I utilized ReconSpider.py, a Python-based web crawler, to crawl the site and extract email addresses. The steps were:

Downloaded the script:
bash
Copy code
wget https://academy.hackthebox.com/storage/modules/279/ReconSpider.zip
unzip ReconSpider.zip
Ran the crawler:
bash
Copy code
python3 ReconSpider.py http://inlanefreight.htb:40391
Methodology:
ReconSpider recursively crawled the website and extracted useful information such as email addresses, comments, and links.
After the crawl, I inspected the generated results.json file for email addresses:
bash
Copy code
cat results.json | jq '.emails'
Question 5: What is the API key the inlanefreight.htb developers will be changing to?
Tools and Command:
Using the same crawler (ReconSpider.py), I examined comments in the results.json file for any indications of future updates. The command:

bash
Copy code
cat results.json | jq '.comments'

## Summary and Lessons Learned

### Summary
This assessment reinforced the importance of a systematic approach to reconnaissance and information gathering in cybersecurity. By leveraging tools like `whois`, `curl`, `gobuster`, and `ReconSpider`, I successfully extracted critical information about the target domain, **inlanefreight.htb**. Each tool played a distinct role in uncovering DNS records, server software, hidden directories, and sensitive data, demonstrating the effectiveness of combining manual techniques with automated tools. The methodology not only allowed me to solve the assessment questions but also highlighted the value of persistence and attention to detail in penetration testing.

### Lessons Learned
1. **Effective Use of Tools:**
   - Understanding the strengths of each tool (e.g., `whois` for domain details, `curl` for HTTP headers) is essential to optimize the reconnaissance process.
   - Automation tools like `ReconSpider` can save significant time while uncovering hidden details that manual analysis might miss.

2. **Importance of Systematic Enumeration:**
   - Following a structured approach—starting from DNS resolution and HTTP inspection to directory enumeration—ensures comprehensive coverage of the target.

3. **Recognizing Hidden Insights:**
   - Comments in HTML source and robots.txt files can contain valuable information, such as developer notes or restricted paths.

4. **Critical Role of the `/etc/hosts` File:**
   - Properly mapping custom domains like `inlanefreight.htb` to IP addresses ensures that tools can resolve and interact with the target effectively.


