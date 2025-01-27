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

Let me know if you'd like more examples or explanations of specific WFuzz functionalities!
