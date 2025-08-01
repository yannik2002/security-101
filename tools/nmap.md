# Nmap

## Table of Contents

1. [Introduction](#introduction)
2. [Host Discovery](#host-discovery)
3. [Port Scanning](#port-scanning)
4. [Version Detection](#version-detection)
5. [Timing Control](#timing-control)
6. [Output Control](#output-control)

## Introduction

Nmap (Network Mapper) is a powerful open-source network scanner first published in 1997 that has become the de facto standard for network discovery and security auditing. Whether you need to discover live devices on a network or identify running services on specific hosts, Nmap provides an efficient and flexible solution that far surpasses manual approaches.

Traditional methods like pinging 254 IP addresses individually or using telnet to test thousands of ports are time-consuming and unreliable. Firewalls can block ICMP traffic, and tools like arp-scan only work within the same network segment. Nmap overcomes these limitations with advanced scanning techniques, stealth options, and comprehensive service detection capabilities.

From simple host discovery on a 192.168.0.1/24 network to detailed service enumeration and vulnerability assessment, Nmap adapts to various scenarios and network configurations. Its extensive feature set makes it an essential tool for network administrators, security professionals, and penetration testers who need reliable network reconnaissance capabilities.

## Host Discovery

Discover live hosts on networks using Nmap's sophisticated detection methods.

### Target Specification

| Format | Example | Description |
|--------|---------|-------------|
| IP range | `192.168.0.1-10` | Scan range of IPs |
| Subnet | `192.168.0.1/24` | Scan entire subnet (0-255) |
| Hostname | `example.com` | Scan by domain name |

### Discovery Methods

**Ping Scan (`-sn`):**
- Discovers live hosts without port scanning
- Low noise, fast discovery
- Different techniques for local vs remote networks

**Local Networks:**
```bash
# Scan local network (same subnet)
nmap -sn 192.168.66.0/24
```
- Uses ARP requests
- Shows MAC addresses and vendors
- Fast and reliable on same network segment

**Remote Networks:**
```bash
# Scan remote network (across routers)
nmap -sn 192.168.11.0/24
```
- Uses ICMP echo, TCP SYN/ACK probes
- Cannot use ARP across routers
- Multiple probe types for reliability

### Additional Options

```bash
# List targets without scanning
nmap -sL 192.168.0.1/24

# Custom discovery probes (advanced)
nmap -PS80,443 target    # TCP SYN to specific ports
nmap -PA80,443 target    # TCP ACK to specific ports
nmap -PU53,161 target    # UDP to specific ports
```

**Note:** Run as root/sudo for full capabilities. Non-root users are limited to basic scans.

## Port Scanning

Discover network services listening on TCP/UDP ports (both protocols support 65,535 ports each).

### Scan Types

| Option | Scan Type | Description | Stealth Level |
|--------|-----------|-------------|---------------|
| `-sT` | TCP Connect | Complete 3-way handshake | Low (logged) |
| `-sS` | TCP SYN | Half-open scan, send SYN only | High (stealthy) |
| `-sU` | UDP Scan | Scan UDP services | Medium |

### TCP Scanning

**Connect Scan (-sT):**
```bash
# Full TCP connection scan
nmap -sT target
```
- Completes full TCP handshake
- Reliable but easily detected/logged
- Default for non-root users

**SYN Scan (-sS):**
```bash
# Stealth SYN scan (requires root)
sudo nmap -sS target
```
- Sends SYN, doesn't complete handshake
- Stealthier, fewer logs generated
- Default for root users

### UDP Scanning

```bash
# Scan UDP services
sudo nmap -sU target

# Common UDP services
sudo nmap -sU -p 53,67,68,69,161,162 target
```
- Slower than TCP scanning
- Useful for DNS, DHCP, SNMP, NTP services
- Closed ports return ICMP unreachable

### Port Range Options

| Option | Ports Scanned | Use Case |
|--------|---------------|----------|
| Default | Top 1000 | Balanced scan |
| `-F` | Top 100 | Fast discovery |
| `-p 22,80,443` | Specific ports | Targeted scan |
| `-p 1-1024` | Port range | System ports |
| `-p-` | All 65535 | Comprehensive scan |

### Practical Examples

```bash
# Quick TCP scan of common ports
nmap -F target

# Comprehensive stealth scan
sudo nmap -sS -p- target

# Scan web services
nmap -p 80,443,8080,8443 target

# Combined TCP and UDP scan
sudo nmap -sS -sU -p 1-1000 target
```

## Version Detection

Extract detailed information about operating systems and service versions.

### Detection Options

| Option | Purpose | Information Gathered |
|--------|---------|---------------------|
| `-O` | OS Detection | Operating system and version |
| `-sV` | Service Version | Service names and versions |
| `-A` | Aggressive | OS + Service + Scripts + Traceroute |
| `-Pn` | Skip Host Discovery | Scan hosts that appear down |

### OS Detection

```bash
# Detect operating system
sudo nmap -O target

# Example output shows:
# Running: Linux 4.X|5.X
# OS details: Linux 4.15 - 5.8
```
- Uses TCP/IP stack fingerprinting
- Provides educated guesses, not 100% accurate
- Requires root privileges for raw packet access

### Service Version Detection

```bash
# Detect service versions
nmap -sV target

# Example output shows:
# 22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.10
# 80/tcp open  http    Apache httpd 2.4.41
```
- Identifies specific service software and versions
- Useful for vulnerability assessment
- Works without root privileges

### Aggressive Scanning

```bash
# Comprehensive scan with all detection features
nmap -A target

# Equivalent to: -O -sV --script=default --traceroute
```
- Combines OS detection, version scanning, default scripts
- More intrusive and noisy
- Provides maximum information

### Practical Examples

```bash
# Quick service detection
nmap -sV -F target

# Stealth scan with OS detection
sudo nmap -sS -O target

# Force scan hosts that appear down
nmap -Pn -sV target

# Complete reconnaissance
sudo nmap -A -p- target

# Target specific services
nmap -sV -p 22,80,443 target
```

### Important Notes

- OS detection requires root/admin privileges
- Service detection works for any user
- `-Pn` useful when ICMP is blocked
- Aggressive scans (`-A`) are easily detected

## Timing Control

Control scan speed to evade detection or optimize performance.

### Timing Templates

| Template | Name | Speed | Use Case | Duration Example* |
|----------|------|-------|----------|------------------|
| `-T0` | Paranoid | Slowest | IDS evasion | 9.8 hours |
| `-T1` | Sneaky | Very slow | Stealth scanning | 27.5 minutes |
| `-T2` | Polite | Slow | Avoid network load | 40.6 seconds |
| `-T3` | Normal | Default | Balanced approach | 0.15 seconds |
| `-T4` | Aggressive | Fast | Quick scanning | 0.13 seconds |
| `-T5` | Insane | Fastest | Very fast networks | Even faster |

*Duration for scanning 100 common ports - varies by network

### Advanced Timing Options

**Parallelism Control:**
```bash
# Control parallel probe count
nmap --min-parallelism 10 --max-parallelism 100 target

# Default: Auto-adjusts based on network performance
```

**Rate Control:**
```bash
# Control packet rate (packets/second)
nmap --min-rate 50 --max-rate 300 target

# Applied to entire scan, not per host
```

**Timeout Control:**
```bash
# Maximum time to wait for slow hosts
nmap --host-timeout 5m target

# Useful for slow networks or unresponsive hosts
```

### Practical Examples

```bash
# Stealth scan for IDS evasion
nmap -T0 -sS target

# Fast aggressive scan
nmap -T4 -A -F target

# Polite scan to avoid network impact
nmap -T2 -sV target

# Custom timing with rate limits
nmap -T3 --max-rate 100 --min-parallelism 5 target

# Quick scan with timeout
nmap -T4 --host-timeout 30s target
```

### Key Considerations

- **T0-T2**: Stealth but slow, good for evasion
- **T3**: Default balanced timing
- **T4-T5**: Fast but easily detected
- **Parallelism**: Auto-adjusts to network conditions
- **Rate limits**: Apply to entire scan duration
- **Timeouts**: Prevent hanging on slow hosts

## Output Control

Control scan information display and save results in various formats.

### Verbosity Levels

| Option | Level | Description | Use Case |
|--------|-------|-------------|----------|
| Default | 0 | Minimal output | Quick results |
| `-v` | 1 | Verbose | Progress updates |
| `-vv` | 2 | Very verbose | Detailed progress |
| `-vvv` | 3+ | Extra verbose | Maximum detail |
| `-d` | Debug | Debug output | Troubleshooting |
| `-d9` | Max debug | All debug info | Deep analysis |

### Verbosity Examples

```bash
# Default output (minimal)
nmap target

# Verbose output (recommended)
nmap -v target

# Very verbose output
nmap -vv target

# Debug output
nmap -d target

# Maximum debug (very detailed)
nmap -d9 target
```

**Verbose output shows:**
- Scan stages (ARP ping, DNS resolution, port scan)
- Real-time progress updates
- Packet statistics
- Timing information

### Output Formats

| Option | Format | Extension | Description |
|--------|--------|-----------|-------------|
| `-oN file` | Normal | `.nmap` | Human-readable text |
| `-oX file` | XML | `.xml` | Machine-parseable XML |
| `-oG file` | Grepable | `.gnmap` | grep/awk friendly |
| `-oA basename` | All formats | All three | Creates all format files |

### Saving Results

```bash
# Save in normal format
nmap -sS target -oN scan_results.nmap

# Save in XML format
nmap -sS target -oX scan_results.xml

# Save in grepable format
nmap -sS target -oG scan_results.gnmap

# Save in all formats (recommended)
nmap -sS target -oA scan_results
# Creates: scan_results.nmap, scan_results.xml, scan_results.gnmap
```

### Practical Examples

```bash
# Verbose scan with all output formats
nmap -v -sS -A target -oA detailed_scan

# Quick scan with verbose output
nmap -v -F target -oN quick_scan.txt

# Debug problematic scan
nmap -d -sS target

# Monitor long-running scan progress
nmap -v -T2 -p- target -oA comprehensive_scan
```

### Interactive Controls

During scan execution:
- Press `v` to increase verbosity
- Press `d` to increase debug level
- `Ctrl+C` to stop scan gracefully
