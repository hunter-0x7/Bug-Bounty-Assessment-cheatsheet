# Mobile Application Security Cheatsheet

This section covers core mobile application security assessment categories, aligned with the **OWASP Mobile Top 10 (2016)**, with platform-specific notes for **iOS** and **Android**.

## Table of Contents

- [OWASP 2016](#owasp-2016)
  - [M1 - Improper Platform Usage](#m1---improper-platform-usage)
  - [M2 - Insecure Data Storage](#m2---insecure-data-storage)
  - [M3 - Insecure Communication](#m3---insecure-communication)
  - [M4 - Insecure Authentication](#m4---insecure-authentication)
  - [M5 - Insufficient Cryptography](#m5---insufficient-cryptography)
  - [M6 - Insecure Authorization](#m6---insecure-authorization)
  - [M7 - Client Code Quality](#m7---client-code-quality)
  - [M8 - Code Tampering](#m8---code-tampering)
  - [M9 - Reverse Engineering](#m9---reverse-engineering)
  - [M10 - Extraneous Functionality](#m10---extraneous-functionality)
- [iOS](#ios)
- [Android](#android)

---

## OWASP 2016

### M1 - Improper Platform Usage
Misuse of platform features, APIs, or security controls (e.g., permissions, Touch ID/Face ID handling, keychain/keystore misuse).

### M2 - Insecure Data Storage
Sensitive data is stored insecurely on-device (e.g., logs, shared preferences, local DB, cache, backups).

### M3 - Insecure Communication
Data exposure in transit due to weak TLS configuration, lack of certificate pinning, or insecure protocols.

### M4 - Insecure Authentication
Weak or broken authentication mechanisms in mobile login/session workflows.

### M5 - Insufficient Cryptography
Weak algorithms, poor key management, improper encryption implementations.

### M6 - Insecure Authorization
Broken access controls allowing users to perform unauthorized actions or access unauthorized data.

### M7 - Client Code Quality
Code-level vulnerabilities in app logic (e.g., memory corruption, insecure input handling, exploitable bugs).

### M8 - Code Tampering
Lack of protections against runtime tampering, repackaging, hooking, and patching.

### M9 - Reverse Engineering
Insufficient hardening/obfuscation makes extraction and analysis of app logic/secrets easier.

### M10 - Extraneous Functionality
Hidden backdoors, test interfaces, debug code, hardcoded credentials, or unnecessary exposed components.

---

## iOS

> Add iOS-specific testing notes, tools, and common findings here.

Example:
- Keychain misuse checks
- ATS/TLS validation
- URL scheme abuse
- Jailbreak detection bypass testing
- IPA static and dynamic analysis

---

## Android

> Add Android-specific testing notes, tools, and common findings here.

Example:
- Manifest/exported component review
- Insecure SharedPreferences/SQLite storage checks
- Network Security Config analysis
- Root detection bypass testing
- APK reverse engineering and runtime instrumentation


# Physical Security Testing Checklist

## Testing: Communication with Mobile and Web Applications

### 1) Reverse Engineer the Mobile Application

#### 1.1 Search for ports being used
**Tools:** JADx, APKTool

- Identify hardcoded port numbers (`80`, `443`, `8080`, `1883`, etc.)
- Trace socket/API client initialization
- Locate fallback ports and alternate endpoints
- Map local vs. remote communication ports

#### 1.2 Search for hardcoded firmware download URLs
**Tools:** JADx, APKTool

- Find OTA/firmware download endpoints
- Detect non-TLS firmware links (`http://`)
- Identify update manifest and version-check URLs
- Enumerate backup/mirror firmware paths

#### 1.3 Identify command messaging format
**Tools:** JADx, APKTool

- Determine payload format (JSON, protobuf, binary, base64)
- Identify command names, action IDs, opcodes
- Locate serializer/deserializer implementation
- Review message integrity controls (signatures/checksums)

#### 1.4 Search for hardcoded SSIDs
**Tools:** JADx, APKTool

- Locate default/provisioning SSID values
- Identify setup-mode naming patterns
- Trace where SSIDs are validated or trusted in logic

#### 1.5 Search for hardcoded encryption keys
**Tools:** JADx, APKTool

- Search for embedded AES/RSA keys
- Identify static IVs, salts, and nonces
- Detect key reuse across build variants
- Flag weak/legacy cryptographic primitives

---

### 2) Intercept the Traffic

#### 2.1 Search and analyze traffic between devices

**Capture paths:**
- Mobile App ↔ Backend API
- Mobile App ↔ Device (LAN/BLE, where applicable)
- Device ↔ Cloud (if observable)

**Analyze for:**
- Authentication and session token handling flaws
- Cleartext traffic or TLS misconfiguration
- Replay vulnerabilities (missing nonce/timestamp)
- Broken authorization in command execution
- Sensitive data leakage in headers, params, or body

---

## Suggested Evidence to Collect

- Decompiled code screenshots/snippets (class + method names)
- Extracted endpoints and port mapping table
- Sample decoded command messages
- Traffic captures showing insecure patterns
- Reproduction steps and security impact notes

## Reporting Tips

For each finding, include:
1. **Title**
2. **Affected component** (mobile app / API / device / firmware update path)
3. **Steps to reproduce**
4. **Observed behavior**
5. **Expected secure behavior**
6. **Impact**
7. **Recommendation**
8. **Evidence**
