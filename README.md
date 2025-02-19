# WFUZZ_Help
Here’s a complete breakdown of **WFuzz** help options with examples to help you use it effectively. WFuzz is a highly configurable tool, primarily used for brute-forcing web applications (e.g., directories, files, parameters).

## **Basic Syntax**
```bash
wfuzz [options] <URL>
```
`FUZZ` is the placeholder for the fuzzing location in the URL.

---

### **1. Payload Options**
Payloads define the type of data to be injected into the `FUZZ` placeholder.

| Option               | Description                                                                                     | Example                                                                 |
|----------------------|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `-z`                | Specify the payload type (e.g., wordlist, range, etc.)                                          | `-z file,/usr/share/wordlists/common.txt`                              |
| `--payloads`         | Show a list of available payloads                                                              | `wfuzz --payloads`                                                     |
| `--hh`              | Hide responses with a specific HTTP response size                                              | `--hh 1345`                                                            |

#### **Example: Using a wordlist as payload**
```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt http://target.com/FUZZ
```

#### **Example: Numeric range payload**
Fuzz with numbers between 1 and 100:
```bash
wfuzz -c -z range,1-100 http://target.com/FUZZ
```

---

### **2. Filter Options**
Filters allow you to narrow down the results by hiding irrelevant responses.

| Option               | Description                                                                                     | Example                                                                 |
|----------------------|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `--hl`              | Hide responses with a specific line count                                                       | `--hl 10`                                                              |
| `--hc`              | Hide responses with a specific HTTP status code                                                 | `--hc 404`                                                             |
| `--hw`              | Hide responses with a specific word count                                                       | `--hw 50`                                                              |
| `--hh`              | Hide responses with a specific content size (in bytes)                                          | `--hh 1345`                                                            |

#### **Example: Hide 404 responses**
```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt --hc 404 http://target.com/FUZZ
```

#### **Example: Filter by word count**
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --hw 50 http://target.com/FUZZ
```

---

### **3. Custom Headers**
Use custom headers for advanced fuzzing scenarios.

| Option               | Description                                                                                     | Example                                                                 |
|----------------------|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `-H`                | Add a custom header                                                                             | `-H "User-Agent: CustomAgent"`                                         |
| `--cookie`          | Add a cookie to the request                                                                     | `--cookie "PHPSESSID=abc123"`                                          |
| `--auth`            | Use basic authentication                                                                        | `--auth user:password`                                                 |

#### **Example: Adding custom headers**
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt -H "User-Agent: CustomAgent" http://target.com/FUZZ
```

#### **Example: Fuzz with a cookie**
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --cookie "SESSIONID=12345" http://target.com/FUZZ
```

---

### **4. POST Data**
Send POST requests with fuzzed data.

| Option               | Description                                                                                     | Example                                                                 |
|----------------------|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `-d`                | Specify POST data                                                                               | `-d "username=FUZZ&password=admin"`                                    |

#### **Example: Fuzzing POST parameters**
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt -d "username=FUZZ&password=admin" http://target.com/login
```

---

### **5. Multi-Position Fuzzing**
Fuzz multiple positions in the URL or payload.

| Option               | Description                                                                                     | Example                                                                 |
|----------------------|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `FUZ2`, `FUZ3`, etc. | Add additional fuzz positions in the URL                                                       | `http://target.com/FUZZ/resource/FUZ2`                                 |

#### **Example: Multiple positions with two payloads**
```bash
wfuzz -c -z file,usernames.txt -z file,passwords.txt http://target.com/login?username=FUZZ&password=FUZ2
```

---

### **6. Output Options**
Control how Wfuzz displays results.

| Option               | Description                                                                                     | Example                                                                 |
|----------------------|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `-o`                | Specify the output format (e.g., JSON, CSV)                                                     | `-o json`                                                              |
| `-v`                | Set verbosity level                                                                             | `-v 2`                                                                 |
| `--output`          | Save results to a file                                                                          | `--output results.txt`                                                 |

#### **Example: Save results as JSON**
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --hc 404 -o json --output results.json http://target.com/FUZZ
```

---

### **7. Recursive Fuzzing**
Automatically fuzz discovered directories or files.

#### **Example: Recursive fuzzing for directories**
```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt --recursion http://target.com/FUZZ
```

---

### **8. Advanced Features**
| Option               | Description                                                                                     | Example                                                                 |
|----------------------|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `--sc`              | Show only specific HTTP status codes                                                            | `--sc 200`                                                             |
| `--follow`          | Follow HTTP redirects                                                                           | `--follow`                                                             |
| `--delay`           | Add a delay between requests                                                                    | `--delay 2`                                                            |
| `--threads`         | Specify the number of concurrent threads                                                        | `--threads 20`                                                         |

#### **Example: Show only HTTP 200 responses**
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --sc 200 http://target.com/FUZZ
```

#### **Example: Add a delay between requests**
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --delay 1 http://target.com/FUZZ
```

---

### **9. DNS Fuzzing**
Fuzz subdomains using a wordlist:
```bash
wfuzz -c -z file,/usr/share/wordlists/dns/subdomains-top1million-20000.txt -H "Host: FUZZ.target.com" http://target.com
```

---

### **10. Recursive Directory Fuzzing**
Automatically fuzz discovered directories:
```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt --recursion http://target.com/FUZZ
```

---

### **11. Proxy Support**
Use a proxy to route requests:
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --proxy http://127.0.0.1:8080 http://target.com/FUZZ
```

---
---

Here are **additional advanced Wfuzz examples** tailored for various scenarios and attack techniques. These cover diverse use cases such as API fuzzing, token discovery, advanced filtering, and more.

---

## **1. Fuzzing Query Parameters**
Discover hidden or vulnerable query parameters in a URL:
```bash
wfuzz -c -z file,/usr/share/wordlists/parameters.txt --hc 404 http://target.com/page.php?FUZZ=value
```

---

## **2. Fuzzing API Endpoints**
Identify hidden endpoints in REST APIs:
```bash
wfuzz -c -z file,/usr/share/wordlists/api-endpoints.txt --hc 404 http://target.com/api/FUZZ
```

---

## **3. JSON POST Data Fuzzing**
Fuzz JSON payloads often used in modern APIs:
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt -d '{"username":"FUZZ","password":"admin"}' -H "Content-Type: application/json" http://target.com/api/login
```

---

## **4. Header Injection Fuzzing**
Discover possible vulnerabilities in HTTP headers:
```bash
wfuzz -c -z file,/usr/share/wordlists/payloads.txt -H "X-Custom-Header: FUZZ" http://target.com
```

---

## **5. Multi-Position Fuzzing in POST Data**
Fuzz multiple fields in POST data:
```bash
wfuzz -c -z file,usernames.txt -z file,passwords.txt -d "username=FUZZ&password=FUZ2" http://target.com/login
```

---

## **6. Token Discovery**
Find sensitive tokens in a URL path:
```bash
wfuzz -c -z file,/usr/share/wordlists/tokens.txt --hc 404 http://target.com/private/FUZZ
```

---

## **7. Recursive Fuzzing for Hidden Content**
Fuzz for directories, then recursively fuzz the discovered directories:
```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt --recursion --hc 404 http://target.com/FUZZ
```

---

## **8. Fuzzing for Subdomains**
Use DNS wordlists to find subdomains:
```bash
wfuzz -c -z file,/usr/share/wordlists/dns/subdomains-top1million-5000.txt -H "Host: FUZZ.target.com" http://target.com
```

---

## **9. Brute-Force Login**
Fuzz both username and password fields simultaneously:
```bash
wfuzz -c -z file,/usr/share/wordlists/usernames.txt -z file,/usr/share/wordlists/passwords.txt --hc 200 -d "username=FUZZ&password=FUZ2" http://target.com/login
```

---

## **10. Filter Responses by Status Code**
Focus only on specific HTTP response codes:
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --sc 200 http://target.com/FUZZ
```

---

## **11. Filtering Results by Content**
Filter responses based on content length, words, or lines.

#### Example: Filter by Content Length
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --hh 500 http://target.com/FUZZ
```

#### Example: Filter by Number of Words
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --hw 30 http://target.com/FUZZ
```

---

## **12. Delay Between Requests**
Introduce delays between requests to avoid detection or rate-limiting:
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --delay 2 http://target.com/FUZZ
```

---

## **13. Advanced Multi-Payload Fuzzing**
Fuzz combinations of usernames, passwords, and tokens:
```bash
wfuzz -c -z file,usernames.txt -z file,passwords.txt -z file,tokens.txt --hc 404 http://target.com/login?user=FUZZ&pass=FUZ2&token=FUZ3
```

---

## **14. Blind Command Injection**
Fuzz vulnerable parameters for command injection by observing response differences:
```bash
wfuzz -c -z file,/usr/share/wordlists/command-injection.txt --hc 404 http://target.com/page.php?cmd=FUZZ
```

---

## **15. SSRF Payload Discovery**
Fuzz SSRF (Server-Side Request Forgery) vectors:
```bash
wfuzz -c -z file,/usr/share/wordlists/ssrf-payloads.txt --hc 404 http://target.com/resource?url=FUZZ
```

---

## **16. Proxy Support for BurpSuite**
Route requests through BurpSuite for deeper analysis:
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --proxy http://127.0.0.1:8080 http://target.com/FUZZ
```

---

## **17. Fuzzing for Open Redirects**
Check if a parameter is vulnerable to open redirection:
```bash
wfuzz -c -z file,/usr/share/wordlists/open-redirect.txt --hc 404 http://target.com/page.php?redirect=FUZZ
```

---

## **18. Brute-Force HTTP Methods**
Fuzz HTTP methods for unauthorized access:
```bash
wfuzz -c -z list,GET-POST-PUT-DELETE http://target.com/resource -X FUZZ
```

---

## **19. Test SQL Injection Vectors**
Inject SQL payloads into parameters:
```bash
wfuzz -c -z file,/usr/share/wordlists/sql-injection.txt --hc 404 http://target.com/page.php?id=FUZZ
```

---

## **20. Fuzzing Cookies**
Inject payloads into cookies to find vulnerabilities:
```bash
wfuzz -c -z file,/usr/share/wordlists/payloads.txt --cookie "SESSION=FUZZ" http://target.com
```

---

## **21. Debugging Responses**
Save all responses for later inspection:
```bash
wfuzz -c -z file,/usr/share/wordlists/common.txt --hc 404 --output results.txt http://target.com/FUZZ
```

---

## **22. Reverse Payload Fuzzing**
Reverse payloads before fuzzing (useful for wordlist variations):
```bash
rev /usr/share/wordlists/common.txt > reversed.txt
wfuzz -c -z file,reversed.txt http://target.com/FUZZ
```

---

## **23. Fuzz for Backup Files**
Search for backup file extensions:
```bash
wfuzz -c -z list,.zip-.tar.gz-.bak-.sql http://target.com/FUZZ
```

---

## **24. Fuzzing Specific HTTP Headers**
Fuzz HTTP headers for vulnerabilities:
```bash
wfuzz -c -z file,/usr/share/wordlists/payloads.txt -H "X-Forwarded-For: FUZZ" http://target.com
```

---

## **25. Recursive Directory and File Discovery**
Combine recursive fuzzing with backup file detection:
```bash
wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt --recursion http://target.com/FUZZ -z list,.bak-.zip-.sql
```

---

These examples cover advanced use cases and can be adapted for penetration testing tasks. Let me know if you'd like detailed explanations or further assistance with specific scenarios!
