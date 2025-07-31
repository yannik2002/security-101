# Tcpdump

## Table of Contents

1. [Introduction](#introduction)
2. [Basic Packet Capture](#basic-packet-capture)
3. [Filtering Expressions](#filtering-expressions)
4. [Advanced Filtering](#advanced-filtering)
5. [Displaying Packets](#displaying-packets)

## Introduction

Tcpdump is a powerful command-line packet analyzer that allows you to capture and examine network traffic in real-time. Originally developed for Unix-like systems in the late 1980s, tcpdump provides essential visibility into network protocol conversations that are normally hidden behind user interfaces.

Understanding network protocols requires seeing the actual packet exchanges - ARP queries, TCP handshakes, and protocol negotiations. Tcpdump makes this possible by capturing live network traffic and displaying packet details, helping security professionals and network administrators understand how networks actually function.

Built on the stable libpcap library (ported to Windows as winpcap), tcpdump offers optimal performance for network analysis and serves as the foundation for many modern networking tools.

## Basic Packet Capture

### Essential Options

| Command | Purpose |
|---------|---------|
| `-i INTERFACE` | Specify network interface (use `ip a s` to list) |
| `-w FILE.pcap` | Save packets to file for later analysis |
| `-r FILE.pcap` | Read packets from saved file |
| `-c COUNT` | Limit number of packets captured |
| `-n` | Don't resolve IP addresses (faster) |
| `-nn` | Don't resolve IPs or port numbers |
| `-v/-vv/-vvv` | Increase verbosity level |

### Quick Examples

```bash
# Capture 10 packets on any interface without resolution
sudo tcpdump -i any -c 10 -nn

# Save traffic to file
sudo tcpdump -i eth0 -w capture.pcap

# Read and analyze saved file
tcpdump -r capture.pcap -n

# Verbose capture with count limit
sudo tcpdump -i ens5 -c 5 -v
```

## Filtering Expressions

Tcpdump becomes powerful when you apply filters to capture only the traffic you're interested in. Without filters, you'll be overwhelmed by the volume of network packets.

### Basic Filters

**By Host:**
- `host IP` or `host HOSTNAME` - captures packets to/from specific host
- `src host IP` - packets from source host only  
- `dst host IP` - packets to destination host only

**By Port:**
- `port PORT_NUMBER` - captures packets to/from specific port
- `src port PORT_NUMBER` - packets from source port only
- `dst port PORT_NUMBER` - packets to destination port only

**By Protocol:**
- `tcp`, `udp`, `icmp`, `ip`, `ip6` - filter by protocol type

### Logical Operators

- `and` - both conditions must be true
- `or` - either condition can be true  
- `not` - condition must be false

### Practical Examples

```bash
# Capture SSH traffic
sudo tcpdump -i any tcp port 22

# Capture DNS queries
sudo tcpdump -i eth0 port 53 -n

# Capture HTTPS traffic to specific site
sudo tcpdump host example.com and tcp port 443 -w https.pcap

# Capture all traffic except TCP
sudo tcpdump not tcp

# Read from capture file and count packets
tcpdump -r traffic.pcap src host 192.168.1.1 -n | wc -l
```

## Advanced Filtering

### Packet Size Filtering

Filter packets by size for performance analysis or detecting large transfers:

- `greater LENGTH` - packets >= specified length
- `less LENGTH` - packets <= specified length

### Header Byte Filtering

Access protocol header bytes using `proto[expr:size]` syntax:
- `proto` - protocol (arp, ether, icmp, ip, tcp, udp)
- `expr` - byte offset (0 = first byte)
- `size` - number of bytes (1, 2, or 4; default = 1)

### TCP Flag Filtering

| Flag | Description | Filter Syntax |
|------|-------------|---------------|
| `tcp-syn` | Synchronize | Connection initiation |
| `tcp-ack` | Acknowledge | Acknowledgment |
| `tcp-fin` | Finish | Connection termination |
| `tcp-rst` | Reset | Connection reset |
| `tcp-push` | Push | Immediate data delivery |

### TCP Flag Examples

```bash
# Packets with only SYN flag set
tcpdump "tcp[tcpflags] == tcp-syn"

# Packets with at least SYN flag set
tcpdump "tcp[tcpflags] & tcp-syn != 0"

# Packets with SYN or ACK flags set
tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"

# Only RST flag set
tcpdump "tcp[tcpflags] == tcp-rst"

# Large packets (>15000 bytes)
tcpdump "greater 15000"

# Small packets with specific host
tcpdump "less 100 and host 192.168.1.1"
```

## Displaying Packets

Customize packet output format for different analysis needs:

### Display Options

| Option | Purpose | Use Case |
|--------|---------|----------|
| `-q` | Quick/brief output | Simplified packet info |
| `-e` | Include MAC addresses | Layer 2 analysis, ARP/DHCP |
| `-A` | ASCII packet data | Plain-text content analysis |
| `-xx` | Hexadecimal format | Binary data, encrypted content |
| `-X` | Hex + ASCII | Best of both worlds |

### Output Examples

**Default output:**
```
18:59:59.979771 IP 104.18.12.149.https > g5000.45248: Flags [P.], seq 2695955324:2695955349, length 25
```

**Quick output (-q):**
```
18:59:59.979771 IP 104.18.12.149.https > g5000.45248: tcp 25
```

**With MAC addresses (-e):**
```
18:59:59.979771 44:df:65:d8:fe:6c > 02:83:1e:40:5d:17, IPv4, length 91: 104.18.12.149.https > g5000.45248
```

**Hex and ASCII (-X):**
```
0x0000:  4500 004d fbd8 4000 3506 d229 6812 0c95  E..M..@.5..)h...
0x0010:  c0a8 4259 01bb b0c0 a0b1 037c aa3b 357d  ..BY.......|.;5}
```

### Practical Usage

```bash
# Find MAC addresses in ARP traffic
sudo tcpdump -e arp

# Analyze HTTP content in ASCII
tcpdump -A tcp port 80

# Detailed hex analysis of suspicious packets
tcpdump -X host suspicious.domain.com

# Quick overview of network traffic
tcpdump -q -c 100
```
