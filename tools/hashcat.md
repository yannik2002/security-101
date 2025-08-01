# Hashcat

## Table of Contents

1. [Introduction](#introduction)
2. [Basic Usage](#basic-usage)
3. [Attack Modes](#attack-modes)
4. [Common Hash Types](#common-hash-types)
5. [Practical Examples](#practical-examples)
6. [Resources](#resources)

## Introduction

Hashcat is the world's fastest and most advanced password recovery tool, supporting over 300 highly-optimized hashing algorithms. It utilizes GPU acceleration to achieve maximum performance and supports various attack modes including dictionary, brute-force, combinator, and rule-based attacks.

Hashcat is particularly valuable for penetration testing, security auditing, and password strength assessment. It can crack passwords from various sources including system hashes, application hashes, and network protocol captures.

## Basic Usage

### Installation & Setup

```bash
# Install hashcat (example for Ubuntu/Debian)
sudo apt install hashcat

# Windows: Download from official website
# macOS: brew install hashcat

# Verify installation
hashcat --version
```

### Basic Syntax

```bash
hashcat [options] hashfile [mask|wordlist|directory]

# Basic structure:
hashcat -m [hash-type] -a [attack-mode] [hash-file] [wordlist/mask]
```

### Essential Options

| Option | Purpose | Example |
|--------|---------|---------|
| `-m` | Hash type (mode) | `-m 1000` (NTLM) |
| `-a` | Attack mode | `-a 0` (dictionary) |
| `-o` | Output file | `-o cracked.txt` |
| `--show` | Show cracked hashes | `--show` |
| `--force` | Ignore warnings | `--force` |
| `-w` | Workload profile | `-w 3` (high) |

## Attack Modes

| Mode | Type | Description | Usage |
|------|------|-------------|-------|
| `-a 0` | Dictionary | Wordlist attack | `hashcat -m 1000 -a 0 hashes.txt rockyou.txt` |
| `-a 1` | Combinator | Combine two wordlists | `hashcat -m 1000 -a 1 hashes.txt list1.txt list2.txt` |
| `-a 3` | Brute-force | Mask attack | `hashcat -m 1000 -a 3 hashes.txt ?a?a?a?a?a?a` |
| `-a 6` | Hybrid | Wordlist + mask | `hashcat -m 1000 -a 6 hashes.txt words.txt ?d?d?d` |
| `-a 7` | Hybrid | Mask + wordlist | `hashcat -m 1000 -a 7 hashes.txt ?d?d?d words.txt` |

### Mask Attack Characters

| Mask | Character Set | Example |
|------|---------------|---------|
| `?l` | Lowercase letters | a-z |
| `?u` | Uppercase letters | A-Z |
| `?d` | Digits | 0-9 |
| `?s` | Special characters | !@#$%^&* |
| `?a` | All printable ASCII | ?l?u?d?s |
| `?b` | All possible bytes | 0x00-0xFF |

## Common Hash Types

| Hash Type | Mode | Example |
|-----------|------|---------|
| MD5 | 0 | `5d41402abc4b2a76b9719d911017c592` |
| SHA1 | 100 | `aaf4c61ddcc5e8a2dabede0f3b482cd9aea9434d` |
| SHA256 | 1400 | `2cf24bf6...` |
| NTLM | 1000 | Windows password hashes |
| bcrypt | 3200 | `$2a$10$...` |
| Linux SHA512 | 1800 | `/etc/shadow` hashes |
| WPA/WPA2 | 2500/22000 | WiFi handshakes |

**Complete list:** [https://hashcat.net/wiki/doku.php?id=hashcat](https://hashcat.net/wiki/doku.php?id=hashcat)

## Practical Examples

### Dictionary Attack

```bash
# Basic dictionary attack against NTLM hashes
hashcat -m 1000 -a 0 ntlm_hashes.txt rockyou.txt

# With rules for password variations
hashcat -m 1000 -a 0 ntlm_hashes.txt rockyou.txt -r best64.rule
```

### Brute-force Attack

```bash
# 6-character brute-force (all printable ASCII)
hashcat -m 1000 -a 3 hashes.txt ?a?a?a?a?a?a

# 8-digit PIN
hashcat -m 1000 -a 3 hashes.txt ?d?d?d?d?d?d?d?d

# Common password pattern: Word + 3 digits
hashcat -m 1000 -a 6 hashes.txt common_words.txt ?d?d?d
```

### Hash Identification

```bash
# Show already cracked passwords
hashcat -m 1000 --show hashes.txt

# Resume interrupted session
hashcat --session mysession --restore

# Benchmark performance
hashcat -b
```

### Advanced Usage

```bash
# High workload with custom output
hashcat -m 1000 -a 0 hashes.txt rockyou.txt -w 4 -o cracked_passwords.txt

# Increment mask length (4-8 characters)
hashcat -m 1000 -a 3 hashes.txt --increment --increment-min=4 --increment-max=8 ?a?a?a?a?a?a?a?a

# Multiple hash files
hashcat -m 1000 -a 0 hash1.txt hash2.txt wordlist.txt
```

## Performance Tips

```bash
# Workload profiles (-w)
-w 1  # Low (desktop usage)
-w 2  # Default
-w 3  # High (dedicated system)
-w 4  # Nightmare (maximum performance)

# Optimize for your hardware
hashcat --opencl-device-types 1,2  # Use CPU and GPU
hashcat --force  # Ignore hardware warnings
```

## Resources

- **Official Website:** [https://hashcat.net/](https://hashcat.net/)
- **Hash Modes Reference:** [https://hashcat.net/wiki/doku.php?id=hashcat](https://hashcat.net/wiki/doku.php?id=hashcat)
- **Rule-based Attacks:** [https://hashcat.net/wiki/doku.php?id=rule_based_attack](https://hashcat.net/wiki/doku.php?id=rule_based_attack)
- **Mask Attack Guide:** [https://hashcat.net/wiki/doku.php?id=mask_attack](https://hashcat.net/wiki/doku.php?id=mask_attack)
- **Popular Wordlists:**
  - RockYou: Most common wordlist for password cracking
  - SecLists: Comprehensive collection of security wordlists
  - Hashcat Utils: Additional tools for wordlist manipulation

**Note:** Always ensure you have proper authorization before attempting to crack passwords. Use hashcat only for legitimate security testing and educational purposes.
