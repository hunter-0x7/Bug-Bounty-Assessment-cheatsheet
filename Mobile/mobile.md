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
