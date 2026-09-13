# Web Application Security Testing

## Application

### Client-side Controls
- Test transmission of data via client
- Test client-side control over user input
- Test thick-client components

### Access Controls
- Understand the access control requirements
- Testing with multiple accounts
- Testing with limited access
- Test for insecure access control methods
  - GitHub Wiki Edit

### Logic Flaws
- Identify the key attack surface
- Test multistage processes
- Test handling of incomplete input
- Test trust boundaries
- Test transaction logic
- Test for usage misuse

### Authentication Mechanism
- Understand the mechanism
- Test password quality
- Test for username enumeration
  - Forgot password
    - Message output
    - Timing requests
  - Signup page
  - Change username
- Test resilience to password guessing
- Test any account recovery function
- Test any remember me function
- Test any impersonation function
- Test username uniqueness
  - Test username uniqueness test for company internal domains
- Test predictability of auto-generated credentials
- Test for unsafe transmission of credentials
- Test for logic flaws
- Exploit any vulnerabilities to gain unauthorized access

### Session Management Mechanism
- Understand the mechanism
- Test tokens for meaning
- Test tokens for predictability
- Check for insecure transmission of tokens
- Check for disclosure of tokens in logs
- Check mapping of tokens to sessions
- Test session termination
- Check for session fixation
- Check for CSRF
  - XSRFProbe
    - https://github.com/0xInfection/XSRFProbe
  - Blazy
    - https://github.com/s0md3v/Blazy
- Check for cookie scope

### Input-based Vulnerabilities
- Fuzz all request parameters
- Test for SQL injection
  - sqlmap
    - https://github.com/sqlmapproject/sqlmap
  - Blazy
    - https://github.com/s0md3v/Blazy
- Test for XSS and other response injection
  - TPLMap
    - https://github.com/epinna/tplmap
  - XSS Hunter
    - https://github.com/mandatoryprogrammer/xsshunter
  - XSSSniper
    - https://github.com/gbrindisi/xsssniper
  - XSStrike
    - https://github.com/s0md3v/XSStrike
  - RABCDAsm
    - https://github.com/CyberShadow/RABCDAsm
  - JSShell
    - https://github.com/Den1al/JSShell
- Test for OS command injection
  - Commix
- Test for path traversal
  - dotdotpwn
  - Panoptic
- Test for script injection
- Test for file inclusion RFI/LFI
  - fimap
  - LFISuite
- Test for XXE
  - XXEinjector
    - https://github.com/enjoiz/XXEinjector
  - oxml_xxe
    - https://github.com/BuffaloWill/oxml_xxe
  - xxe.sh
    - https://www.xxe.sh/
- Test for DOM based attacks
  - tracy
    - https://github.com/nccgroup/tracy
  - domdig
    - https://github.com/fcavallarin/domdig
- Test for race conditions
  - Race The Web
    - https://github.com/TheHackerDev/race-the-web
- Test for object injection
- Test for HTML injection
- Test for CRLF injection
- Test for host header injection
- Test for RCE
- Test for buffer overflow
- Test for open URL redirects
  - Search for parameters:
    - go
    - return
    - url
    - next
    - redirect
    - page
  - Steal OAuth tokens on Facebook
- Test for reflected file download
  - rfd-checker
- Test for file upload exploits
- Test for SSRF
  - SSRFmap
    - https://github.com/swisskyrepo/SSRFmap
  - Gopherus
    - https://github.com/tarunkant/Gopherus
- Test for parameter tampering

### Configuration Flaws
- CORS configuration
  - ORScanner
    - https://github.com/chenjj/CORScanner
- SOP bypass
- Test for path traversal
- Test for directory traversal
- Test for IDOR

### Misc Check
- Check for frame injection
  - savanttools.com
    - http://savanttools.com/test-frame
  - Blazy
    - https://github.com/s0md3v/Blazy
- Check for local privacy vulnerabilities
- Follow up any information leakage
  - Nmap comments script
- Tabnabbing
- UI redressing (clickjacking)
- Content spoofing
- Homograph attack
- CSP bypass
  - JSONBee
    - https://github.com/zigoo0/JSONBee
- Malware inspection
  - VirusTotal
  - CMS sites
    - Quttera
      - https://quttera.com
    - SUCURI
      - https://sitecheck.sucuri.net/?cjevent=11bc24974ccd11ea828500e90a240612&cj_aid=13948096&cj_pid=8092889&cj_cid=4761150

## SSL and Security Headers Testing
- observatory.mozilla.org
- ssllabs.com
  - https://www.ssllabs.com/ssltest/
- sslscan

## Automated Scanning Tools
- Nikto
- Wapiti
- w3af
- sonarwhal
- lazyrecon
- Burp Pro
- ZAP

## CMS
### Wordpress
- wpscan
- WPSeku

### Joomla!
- joomScan
- joomlavs

### Drupal
- droopescan

### Sharepoint

## Database
- sqlmap
- NoSQLMap
  - https://github.com/codingo/NoSQLMap
- mongoaudit
  - https://github.com/stampery/mongoaudit

## API
### API1 - Broken Object Level Authorization
- Every API endpoint that receives an ID of an object, and performs any type of action on the object, should implement object level authorization checks.
- The checks should validate that the logged-in user does have access to perform the requested action on the requested object.
- Astra
  - https://github.com/flipkart-incubator/Astra
- Test for IDOR

### API2 - Broken Authentication
- Permits credential stuffing whereby the attacker has a list of valid usernames and passwords
- Permits attackers to perform a brute force attack on the same user, without presenting captcha / account lockout mechanism
- Permits weak passwords
- Test for URL sensitive data (password, tokens, api keys)
- Doesn’t validate the authenticity of tokens
- Uses plain text, encrypted, or weakly hashed passwords
- Uses weak encryption keys / API keys

#### JWT
- Test JWT secret brute-forcing
  - jwt_tool
    - https://github.com/ticarpi/jwt_tool
- Test if algorithm could be changed
  - jwt.io
    - https://jwt.io/#debugger-io
- Test token expiration time (TTL, RTTL)
- Test if sensitive data is in the JWT
  - jwt.io
    - https://jwt.io/#debugger-io
- Check for injection in `kid` element
- Check for time constant verification for HMAC
- Check that keys and secrets are different between ENV

#### OAuth
- Test `redirect_uri` for open redirects
- Test the existence of `response_type=token`
- Test CSRF

- Check for Basic Auth

### API3 - Excessive Data Exposure
- The API returns sensitive data to the client by design. This data is usually filtered on the client side before being presented to the user. An attacker can easily sniff the traffic and see the sensitive data

### API4 - Lack of Resources & Rate Limiting
- Execution timeouts
- Test brute-force attacks
- Max allocable memory
- Number of file descriptors
- Number of processes
- Request payload size (e.g. uploads)
- Number of requests per client/resource
  - Astra
    - https://github.com/flipkart-incubator/Astra
- Number of records per page to return in a single request response

### API5 - Broken Function Level Authorization
- Can a regular user access administrative endpoints?
- Testing different HTTP methods (GET, POST, PUT, DELETE, PATCH) will allow level escalation?
- Enumerate/bruteforce endpoints for getting unauthorized requests

### API6 - Mass Assignment
- An API endpoint is vulnerable if it automatically converts client parameters into internal object properties, without considering the sensitivity and the exposure level of these properties. This could allow an attacker to update object properties that they should not have access to.
- Sensitive properties:
  - Permission-related properties: `user.is_admin`, `user.is_vip` should only be set by admins.
  - Process-dependent properties: `user.cash` should only be set internally after payment verification.
  - Internal properties: `article.created_time` should only be set internally by the application.

### API7 - Security Misconfiguration
- Appropriate security hardening is missing across any part of the application stack, or if it has improperly configured permissions on cloud services.
- The latest security patches are missing, or the systems are out of date.
- Unnecessary features are enabled (e.g., HTTP verbs).
- Transport Layer Security (TLS) is missing.
- Security directives are not sent to clients (e.g., Security Headers).
- A Cross-Origin Resource Sharing (CORS) policy is missing or improperly set.
  - Astra
    - https://github.com/flipkart-incubator/Astra
- Error messages include stack traces, or other sensitive information is exposed.

### API8 - Injection
- Client-supplied data is not validated, filtered, or sanitized by the API.
  - Astra
    - https://github.com/flipkart-incubator/Astra
- Client-supplied data is directly used or concatenated to SQL/NoSQL/LDAP queries, OS commands, XML parsers, and Object Relational Mapping (ORM)/Object Document Mapper (ODM).
  - Astra
    - https://github.com/flipkart-incubator/Astra
- Data coming from external systems (e.g., integrated systems) is not validated, filtered, or sanitized by the API.

### API9 - Improper Assets Management
- There is no documentation, or the existing documentation is not updated.
- Hosts inventory is missing or outdated.
- Integrated services inventory, either first- or third-party, is missing or outdated.
- Old or previous API versions are running unpatched.

### API10 - Insufficient Logging & Monitoring
- It does not produce any logs, the logging level is not set correctly, or log messages do not include enough detail.
- Log integrity is not guaranteed (e.g., Log Injection).

## Shodan
- https://www.shodan.io/

## Censys
- https://censys.io/

## BinaryEdge
- https://app.binaryedge.io/

## DNS Zone Transfer
- DIG
