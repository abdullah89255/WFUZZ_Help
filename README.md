# WFUZZ_Help
Here’s a complete breakdown of **WFuzz** help options with examples to help you use it effectively. WFuzz is a highly configurable tool, primarily used for brute-forcing web applications (e.g., directories, files, parameters).

### View Help Menu
Run this command to see the available options:
```bash
wfuzz --help
```

---

### 1. **Basic Command Structure**
The general structure for a WFuzz command is:
```bash
wfuzz [options] -u <URL> -w <wordlist>
```

- **`-u`**: Specifies the target URL.
- **`-w`**: Specifies the wordlist to use.

---

### 2. **Key Options with Examples**

#### **a) URL (-u) and Wordlist (-w)**
Specify the URL and wordlist:
```bash
wfuzz -u http://example.com/FUZZ -w /path/to/wordlist.txt
```
- The placeholder `FUZZ` in the URL is replaced by entries from the wordlist.

#### **b) Filtering Results (-c, --hc, --hl, --hw)**
Filter the output based on the response.

- **`-c`**: Colorize the output.
- **`--hc <code>`**: Hide responses with specific HTTP status codes.
  ```bash
  wfuzz -u http://example.com/FUZZ -w wordlist.txt --hc 404
  ```
  This hides all `404 Not Found` responses.

- **`--hl <length>`**: Hide responses of a specific content length.
  ```bash
  wfuzz -u http://example.com/FUZZ -w wordlist.txt --hl 0
  ```
  This hides responses with a content length of `0`.

- **`--hw <words>`**: Hide responses based on word count.
  ```bash
  wfuzz -u http://example.com/FUZZ -w wordlist.txt --hw 10
  ```
  This hides responses with 10 words in the body.

#### **c) Adding Headers (--hc, --hh)**
Add or modify HTTP headers to customize requests.

- Add headers:
  ```bash
  wfuzz -u http://example.com/FUZZ -w wordlist.txt -H "User-Agent: CustomUserAgent"
  ```
- Add multiple headers:
  ```bash
  wfuzz -u http://example.com/FUZZ -w wordlist.txt -H "Cookie: session=123" -H "Referer: http://google.com"
  ```

#### **d) POST Requests (-d)**
Send data in a POST request:
```bash
wfuzz -u http://example.com/login -w usernames.txt -d "username=FUZZ&password=admin"
```

#### **e) Proxy Support (--proxy)**
Route requests through a proxy server:
```bash
wfuzz -u http://example.com/FUZZ -w wordlist.txt --proxy http://127.0.0.1:8080
```

#### **f) Recursive Fuzzing**
Perform recursive fuzzing on directories:
```bash
wfuzz -u http://example.com/FUZZ/ -w directories.txt
```

#### **g) Multiple Injection Points**
Fuzz multiple points by using `FUZZ` multiple times in the URL:
```bash
wfuzz -u http://example.com/FUZZ/FUZZ -w wordlist.txt
```

#### **h) Output Options**
Save output to a file:
```bash
wfuzz -u http://example.com/FUZZ -w wordlist.txt -o output.txt
```

---

### 3. **Advanced Usage**

#### **a) Use Multiple Wordlists**
Fuzz different points with separate wordlists:
```bash
wfuzz -u http://example.com/FUZZ/FUZZ -w wordlist1.txt -w wordlist2.txt
```

#### **b) Fuzz Cookies**
Fuzz values within cookies:
```bash
wfuzz -u http://example.com/ -w wordlist.txt -b "session=FUZZ"
```

#### **c) Customizing HTTP Methods**
Change the HTTP method to something like `PUT`, `DELETE`, etc.:
```bash
wfuzz -u http://example.com/FUZZ -w wordlist.txt --method PUT
```

---

### 4. **Common Examples**

#### **Directory Brute-Forcing**
```bash
wfuzz -u http://example.com/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

#### **Parameter Fuzzing**
```bash
wfuzz -u http://example.com/page?FUZZ=test -w params.txt
```

#### **File Extension Brute-Forcing**
```bash
wfuzz -u http://example.com/index.FUZZ -w extensions.txt
```

#### **Fuzz Hidden Parameters**
```bash
wfuzz -u http://example.com/page?param=value&FUZZ= -w params.txt
```

#### **SQL Injection**
```bash
wfuzz -u http://example.com/page?id=FUZZ -w sqli.txt
```

---

### 5. **Wordlist Sources**
Popular wordlists can be found in the following locations:
- **Kali Linux**: `/usr/share/wordlists/`
- **SecLists (GitHub)**: [SecLists](https://github.com/danielmiessler/SecLists)

---

Here is a detailed breakdown of all Wfuzz options and their practical examples. Wfuzz is highly customizable and versatile, making it ideal for web application fuzzing.

---

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

Wfuzz is a highly flexible tool that supports complex fuzzing scenarios. Let me know if you'd like more advanced examples or specific use cases!
