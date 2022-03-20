# Apache Path Traversal Exploit

This exploit is based on a few CVE vulnerabilities affecting Apache 2.4.49. We basically use URL-encoded characters to access certain files or access otherwise restricted resources.

## Affected CVEs
1. CVE-2021-41773
    a. A flaw was found in a change made to path normalization in Apache HTTP Server 2.4.49. An attacker could use a path traversal attack to map URLs to files outside the directories configured by Alias-like directives. If files outside of these directories are not protected by the usual default configuration "require all denied", these requests can succeed. If CGI scripts are also enabled for these aliased pathes, this could allow for remote code execution. This issue is known to be exploited in the wild. This issue only affects Apache 2.4.49 and not earlier versions. The fix in Apache HTTP Server 2.4.50 was found to be incomplete, see CVE-2021-42013.

2. CVE-2021-42013
    a. It was found that the fix for CVE-2021-41773 in Apache HTTP Server 2.4.50 was insufficient. An attacker could use a path traversal attack to map URLs to files outside the directories configured by Alias-like directives. If files outside of these directories are not protected by the usual default configuration "require all denied", these requests can succeed. If CGI scripts are also enabled for these aliased pathes, this could allow for remote code execution. This issue only affects Apache 2.4.49 and Apache 2.4.50 and not earlier versions.

## Usage
```bash
$ python3 exploit.py -h

--cgi   Use if Apache server has CGI enabled
--port  Port the Apache server is listening on (Default: 8080)
--path  Path to the file on the Apache server to output (Default: /etc/passwd)
--ip    The website or IP address where the Apache server is hosted (Default: localhost)
-v      Increases verbosity
```

### Full example against a cgi-enabled server running on port 8000
```bash
$ python3 exploit.py --cgi --port 8000 --path "/etc/passwd"
```