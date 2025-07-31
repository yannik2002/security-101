# Networking

## Table of Contents
- [OSI Model Layers](#osi-model-layers)
- [TCP/IP Model](#tcpip-model)
- [IP Addresses and Subnets](#ip-addresses-and-subnets)
- [UDP and TCP](#udp-and-tcp)
- [Protocol Port Reference](#protocol-port-reference)
- [TLS](#tls)
- [HTTPS](#https)
- [SSH](#ssh)
- [VPN](#vpn)
- [Encapsulation](#encapsulation)
- [DHCP](#dhcp)
- [ARP](#arp)
- [ICMP](#icmp)
- [Routing](#routing)
- [NAT](#nat)
- [DNS](#dns)
- [WHOIS](#whois)
- [FTP](#ftp)
- [SMTP](#smtp)
- [POP3](#pop3)
- [IMAP](#imap)

## OSI Model Layers

| Layer Number | Layer Name | Main Function | Example Protocols and Standards |
|--------------|------------|---------------|--------------------------------|
| Layer 7 | Application layer | Providing services and interfaces to applications | HTTP, FTP, DNS, POP3, SMTP, IMAP |
| Layer 6 | Presentation layer | Data encoding, encryption, and compression | Unicode, MIME, JPEG, PNG, MPEG |
| Layer 5 | Session layer | Establishing, maintaining, and synchronising sessions | NFS, RPC |
| Layer 4 | Transport layer | End-to-end communication and data segmentation | UDP, TCP |
| Layer 3 | Network layer | Logical addressing and routing between networks | IP, ICMP, IPSec |
| Layer 2 | Data link layer | Reliable data transfer between adjacent nodes | Ethernet (802.3), WiFi (802.11) |
| Layer 1 | Physical layer | Physical data transmission media | Electrical, optical, and wireless signals |

## TCP/IP Model

| Layer Number | ISO OSI Model | TCP/IP Model (RFC 1122) | Protocols |
|--------------|---------------|-------------------------|-----------|
| 7 | Application Layer | Application Layer | HTTP, HTTPS, FTP, POP3, SMTP, IMAP, Telnet, SSH |
| 6 | Presentation Layer | | |
| 5 | Session Layer | | |
| 4 | Transport Layer | Transport Layer | TCP, UDP |
| 3 | Network Layer | Internet Layer | IP, ICMP, IPSec |
| 2 | Data Link Layer | Link Layer | Ethernet 802.3, WiFi 802.11 |
| 1 | Physical Layer | | |

## IP Addresses and Subnets

### Key Points

**IP Address Structure:**
- IPv4 addresses consist of 4 octets (32 bits total)
- Each octet ranges from 0-255
- Example: 192.168.1.100

**Subnet Notation:**
- Subnet mask 255.255.255.0 = /24 notation
- /24 means first 24 bits define the network portion
- Hosts can range from .1 to .254 (0 and 255 are reserved for network and broadcast)

**Private IP Ranges (RFC 1918):**
- 10.0.0.0 - 10.255.255.255 (10/8)
- 172.16.0.0 - 172.31.255.255 (172.16/12)  
- 192.168.0.0 - 192.168.255.255 (192.168/16)

**Network Commands:**
- Windows: `ipconfig`
- Linux/Unix: `ifconfig` or `ip a s`

**Routing:**
- Routers operate at Layer 3 (Network Layer)
- Forward packets between networks based on IP addresses
- Private addresses need NAT to access the internet

## UDP and TCP

### Key Points

**Port Numbers:**
- Use 2 octets (16 bits)
- Range: 1-65535 (port 0 is reserved)
- Identify specific processes on a host

**Well-Known Ports (1-1024):**
- First 2^10 = 1024 ports
- Reserved for system services and common protocols
- Examples: 21 (FTP), 22 (SSH), 23 (Telnet), 25 (SMTP), 53 (DNS), 80 (HTTP), 110 (POP3), 143 (IMAP), 443 (HTTPS), 993 (IMAPS), 995 (POP3S)
- Require administrative privileges to bind on Unix-like systems

**UDP (User Datagram Protocol):**
- Layer 4 (Transport Layer) protocol
- Connectionless - no connection establishment needed
- No delivery confirmation or acknowledgment
- Fast but unreliable
- Similar to standard mail with no delivery confirmation

**TCP (Transmission Control Protocol):**
- Layer 4 (Transport Layer) protocol  
- Connection-oriented - requires connection establishment
- Reliable data delivery with acknowledgments
- Uses sequence numbers to track data octets
- Detects lost or duplicated packets

**TCP Three-Way Handshake:**
1. **SYN**: Client sends SYN packet with initial sequence number
2. **SYN-ACK**: Server responds with SYN-ACK packet and its sequence number
3. **ACK**: Client sends ACK packet to complete the handshake

**Key Flags:**
- SYN (Synchronize): Used to establish connections
- ACK (Acknowledgment): Used to confirm packet reception

## Protocol Port Reference

### Key Points

**Default Port Numbers:**
This table provides a quick reference for the default port numbers of fundamental network protocols covered in this document.

| Protocol | Transport Protocol | Default Port Number | Secure Version | Secure Port |
|----------|-------------------|-------------------|----------------|-------------|
| TELNET | TCP | 23 | SSH | 22 |
| DNS | UDP or TCP | 53 | DNS over HTTPS (DoH) | 443 |
| HTTP | TCP | 80 | HTTPS | 443 |
| FTP | TCP | 21 | SFTP/FTPS | 22/990 |
| SMTP | TCP | 25 | SMTPS | 465/587 |
| POP3 | TCP | 110 | POP3S | 995 |
| IMAP | TCP | 143 | IMAPS | 993 |
| DHCP | UDP | 67/68 | - | - |

**Protocol Categories:**

**Remote Access:**
- **TELNET (23)**: Unencrypted remote terminal access
- **SSH (22)**: Secure encrypted remote access and file transfer

**Web Services:**
- **HTTP (80)**: Unencrypted web page transfer
- **HTTPS (443)**: Encrypted web page transfer with SSL/TLS

**Email Protocols:**
- **SMTP (25)**: Sending outbound email messages
- **POP3 (110)**: Retrieving email from server to client
- **IMAP (143)**: Synchronized email access across multiple devices

**File Transfer:**
- **FTP (21)**: Unencrypted file transfer with separate data connection
- **SFTP (22)**: Secure file transfer over SSH connection

**Network Services:**
- **DNS (53)**: Domain name to IP address resolution
- **DHCP (67/68)**: Automatic IP address assignment

**Security Notes:**
- Protocols without encryption (TELNET, HTTP, FTP, SMTP, POP3, IMAP) transmit data in plain text
- Always prefer secure alternatives (SSH, HTTPS, SFTP, SMTPS, POP3S, IMAPS) when available
- Secure versions use SSL/TLS encryption to protect data in transit
- Port numbers help firewalls and network administrators control protocol access

## TLS

### Key Points

**Purpose:**
- Transport Layer Security (TLS) provides secure communication over insecure networks
- Ensures confidentiality and integrity of data transmission
- Protects against eavesdropping, tampering, and man-in-the-middle attacks
- Essential for secure web browsing, email, and online transactions

**Historical Development:**
- **SSL 2.0 (1995)**: First public version by Netscape Communications
- **SSL 3.0 (1996)**: Improved security over SSL 2.0
- **TLS 1.0 (1999)**: IETF upgrade to SSL 3.0 with enhanced security
- **TLS 1.3 (2018)**: Major protocol overhaul with significant security improvements
- Over two decades of continuous development and security enhancements

**Security Problem Solved:**
- Before TLS/SSL, all network communication was in plain text
- Attackers could use packet-capturing tools to read passwords, emails, and chats
- Network cards in promiscuous mode could capture all network traffic
- Login credentials and sensitive data transmitted without protection

**TLS Protocol Details:**
- Operates at Layer 4 (Transport Layer) of OSI model
- Cryptographic protocol providing end-to-end security
- Establishes secure channel between client and server
- Uses combination of symmetric and asymmetric encryption

**Key Security Features:**
- **Confidentiality**: Encrypts data to prevent unauthorized reading
- **Integrity**: Detects data modification during transmission
- **Authentication**: Verifies identity of communicating parties
- **Non-repudiation**: Prevents denial of message transmission

**TLS Certificate System:**

**Certificate Signing Request (CSR) Process:**
1. Server administrator creates Certificate Signing Request
2. CSR submitted to Certificate Authority (CA) for verification
3. CA verifies the request and issues digital certificate
4. Signed certificate used to identify server to clients
5. Clients verify certificate validity using CA's public key

**Certificate Authority (CA):**
- Trusted third-party organizations that issue digital certificates
- Examples: DigiCert, Symantec, GlobalSign, Let's Encrypt
- Browsers and operating systems include trusted CA certificates
- Annual fees typically required (except Let's Encrypt - free)

**Certificate Types:**
- **CA-Signed Certificates**: Verified by trusted Certificate Authority
- **Self-Signed Certificates**: Created without third-party verification
- **Extended Validation (EV)**: Highest level of identity verification
- **Domain Validation (DV)**: Basic domain ownership verification

**Protocols Enhanced by TLS:**
- **HTTP → HTTPS**: Secure web browsing (port 443)
- **SMTP → SMTPS**: Secure email sending (port 465/587)
- **POP3 → POP3S**: Secure email retrieval (port 995)
- **IMAP → IMAPS**: Secure email synchronization (port 993)
- **FTP → FTPS**: Secure file transfer (port 990)
- **DNS → DoT**: DNS over TLS (port 853)
- **MQTT → MQTTS**: Secure IoT messaging
- **SIP → SIPS**: Secure VoIP communication

**TLS Handshake Overview:**
1. **Client Hello**: Client initiates connection with supported TLS versions
2. **Server Hello**: Server responds with chosen TLS version and certificate
3. **Certificate Verification**: Client validates server's certificate
4. **Key Exchange**: Secure exchange of encryption keys
5. **Secure Communication**: Encrypted data transmission begins

**Common TLS Versions:**
- **TLS 1.0**: Deprecated due to security vulnerabilities
- **TLS 1.1**: Also deprecated, should not be used
- **TLS 1.2**: Widely supported, still secure for most applications
- **TLS 1.3**: Latest version with improved security and performance

**Security Considerations:**
- Always use latest TLS version when possible (TLS 1.3 preferred)
- Properly validate certificates to prevent man-in-the-middle attacks
- Be cautious with self-signed certificates (cannot verify authenticity)
- Keep CA certificate stores updated for proper certificate validation
- Use strong cipher suites and disable weak encryption algorithms

**Real-World Applications:**
- **Online Banking**: Protects financial transactions and account access
- **E-commerce**: Secures credit card information and personal data
- **Email Communication**: Prevents interception of private messages
- **Remote Work**: Enables secure VPN and remote desktop connections
- **Social Media**: Protects user credentials and private communications

**Without TLS Impact:**
- Online shopping would expose credit card numbers
- Banking credentials visible to network attackers
- Email and messaging completely readable by eavesdroppers
- Modern Internet applications would be unusable for sensitive data

## HTTPS

### Key Points

**Purpose:**
- HTTPS (Hypertext Transfer Protocol Secure) is HTTP over TLS
- Provides secure web communication by encrypting HTTP traffic
- Protects web browsing from eavesdropping and tampering
- Essential for secure online transactions and data protection

**HTTP vs HTTPS Security:**
- **HTTP (Port 80)**: All traffic sent in cleartext (plain text)
- **HTTPS (Port 443)**: All traffic encrypted using TLS
- Attackers can easily read HTTP traffic with packet capture tools
- HTTPS traffic appears as encrypted "gibberish" without decryption keys

**HTTP Connection Process (Insecure):**
1. **DNS Resolution**: Resolve domain name to IP address
2. **TCP Handshake**: Establish TCP three-way handshake with server (port 80)
3. **HTTP Communication**: Send HTTP requests (e.g., `GET / HTTP/1.1`) in plain text

**HTTPS Connection Process (Secure):**
1. **DNS Resolution**: Resolve domain name to IP address
2. **TCP Handshake**: Establish TCP three-way handshake with server (port 443)
3. **TLS Session**: Negotiate and establish TLS encryption session
4. **HTTP Communication**: Send HTTP requests encrypted within TLS tunnel

**Network Traffic Analysis:**

**HTTP Traffic Capture:**
- TCP handshake packets visible (SYN, SYN-ACK, ACK)
- HTTP requests and responses readable in plain text
- GET requests, POST data, cookies, and headers exposed
- Passwords and sensitive data transmitted without protection

**HTTPS Traffic Capture:**
- TCP handshake packets visible (same as HTTP)
- TLS handshake packets visible (certificate exchange, key negotiation)
- Application data packets encrypted ("Application Data" in Wireshark)
- HTTP content completely hidden from network analysis

**TLS Handshake in HTTPS:**
- **Client Hello**: Browser initiates TLS connection
- **Server Hello**: Server responds with certificate and encryption parameters
- **Certificate Verification**: Browser validates server's TLS certificate
- **Key Exchange**: Secure exchange of encryption keys
- **Finished**: TLS tunnel established, HTTP communication begins

**Security Benefits:**
- **Confidentiality**: HTTP requests and responses encrypted
- **Integrity**: Data cannot be modified in transit
- **Authentication**: Server identity verified through certificates
- **Protection**: Prevents password theft, session hijacking, data interception

**Packet Analysis Differences:**

**HTTP Packet Contents (Visible):**
- Request methods (GET, POST, PUT, DELETE)
- URLs and query parameters
- HTTP headers (User-Agent, Cookies, Authentication)
- Form data and uploaded files
- Response content (HTML, JSON, images)

**HTTPS Packet Contents (Encrypted):**
- Only "Application Data" visible in network captures
- No readable HTTP headers or content
- Encrypted payload appears as random bytes
- Requires private key for decryption (impractical for attackers)

**Real-World Security Impact:**

**Without HTTPS (HTTP only):**
- Login credentials transmitted in plain text
- Credit card numbers visible to network sniffers
- Personal information exposed to eavesdroppers
- Session cookies vulnerable to hijacking
- Man-in-the-middle attacks easily executed

**With HTTPS Protection:**
- All sensitive data encrypted during transmission
- Login forms protected from credential theft
- E-commerce transactions secured
- Banking and financial data protected
- Modern security standard for all websites

**HTTPS Certificate Validation:**
- Browser checks server certificate against trusted CAs
- Verifies certificate hasn't expired or been revoked
- Ensures certificate matches the domain name
- Displays security indicators (lock icon, green bar)
- Warns users about invalid or self-signed certificates

**Debugging HTTPS with Encryption Keys:**
- Private keys can decrypt HTTPS traffic for debugging
- Wireshark can decrypt traffic when provided with server's private key
- Decrypted traffic shows normal HTTP requests and responses
- Only possible with access to server's private key (not typical for attackers)

**Protocol Stack Integration:**
- TLS operates between TCP (Layer 4) and HTTP (Layer 7)
- No changes required to TCP or IP protocols
- HTTP operates normally within TLS encryption tunnel
- Seamless integration without modifying existing protocols

**Modern Web Security:**
- HTTPS now standard for all websites (not just sensitive sites)
- Search engines favor HTTPS sites in rankings
- Browsers mark HTTP sites as "Not Secure"
- Certificate authorities provide free certificates (Let's Encrypt)
- HTTP/2 and HTTP/3 require HTTPS by default

## SSH

### Key Points

**Purpose:**
- SSH (Secure Shell) provides secure remote access to systems
- Replaces insecure TELNET protocol with encrypted communication
- Essential for secure system administration and remote management
- Operates at Layer 7 (Application Layer)

**Historical Development:**
- **SSH-1 (1995)**: Developed by Tatu Ylönen as freeware
- **SSH-2 (1996)**: More secure version with improved protocol design
- **OpenSSH (1999)**: Open-source implementation by OpenBSD developers
- Most modern SSH clients based on OpenSSH libraries and source code

**Security Problem Solved:**
- TELNET transmits all traffic including credentials in plain text
- Network monitoring can easily capture login passwords
- SSH encrypts all communication to prevent eavesdropping
- Provides secure alternative to insecure remote access protocols

**SSH vs TELNET Comparison:**
- **TELNET (Port 23)**: Unencrypted, credentials visible to attackers
- **SSH (Port 22)**: Encrypted, secure authentication and communication
- **TELNET**: No integrity protection, vulnerable to tampering
- **SSH**: Cryptographic integrity protection against modification

**Key SSH Benefits:**

**Secure Authentication:**
- **Password Authentication**: Traditional username/password with encryption
- **Public Key Authentication**: Cryptographic key pairs for passwordless login
- **Two-Factor Authentication**: Additional security layer beyond passwords
- **Host Key Verification**: Protects against man-in-the-middle attacks

**Confidentiality:**
- **End-to-End Encryption**: All traffic encrypted between client and server
- **Session Protection**: Commands, output, and data transfers encrypted
- **Credential Protection**: Login passwords never transmitted in plain text
- **Server Key Notifications**: Alerts when server keys change

**Integrity:**
- **Cryptographic Checksums**: Detects any tampering with transmitted data
- **Message Authentication**: Ensures data hasn't been modified in transit
- **Session Integrity**: Prevents injection of malicious commands
- **Data Validation**: Verifies all communication authenticity

**Advanced Features:**

**Tunneling (Port Forwarding):**
- **Local Port Forwarding**: Forward local ports through SSH connection
- **Remote Port Forwarding**: Allow remote access to local services
- **Dynamic Port Forwarding**: SOCKS proxy for secure browsing
- **VPN-like Functionality**: Route other protocols through secure SSH tunnel

**X11 Forwarding:**
- **Graphical Applications**: Run GUI programs on remote systems
- **Display Forwarding**: Show remote applications on local screen
- **Secure Graphics**: Encrypted transmission of graphical interfaces
- **Remote Desktop Capability**: Access full graphical environments

**SSH Connection Commands:**

**Basic Connection:**
- `ssh username@hostname`: Connect with specific username
- `ssh hostname`: Connect using current username
- Password prompt appears for authentication
- Public key authentication logs in immediately (if configured)

**Advanced Connection Options:**
- `ssh hostname -X`: Enable X11 forwarding for graphical applications
- `ssh hostname -L 8080:localhost:80`: Local port forwarding
- `ssh hostname -R 9090:localhost:22`: Remote port forwarding
- `ssh hostname -D 1080`: Dynamic port forwarding (SOCKS proxy)

**SSH Authentication Methods:**

**Password Authentication:**
- Traditional username and password combination
- Encrypted transmission prevents credential theft
- Still vulnerable to brute force attacks
- Generally less secure than key-based authentication

**Public Key Authentication:**
- Uses cryptographic key pairs (public/private keys)
- Private key stored securely on client system
- Public key installed on server for authentication
- Eliminates password transmission and brute force risks

**Key Management:**
- `ssh-keygen`: Generate new SSH key pairs
- `ssh-copy-id`: Install public key on remote server
- `ssh-add`: Add private keys to SSH agent
- `authorized_keys`: Server file containing allowed public keys

**Security Features:**

**Host Key Verification:**
- Each SSH server has unique host key fingerprint
- Client verifies server identity on first connection
- Warns if server key changes (potential man-in-the-middle attack)
- Stores known host keys in `~/.ssh/known_hosts`

**Connection Security:**
- **Perfect Forward Secrecy**: Each session uses unique encryption keys
- **Strong Encryption**: Modern ciphers (AES, ChaCha20) protect data
- **Message Authentication Codes**: Prevent tampering with messages
- **Compression**: Optional data compression for improved performance

**Real-World Applications:**
- **System Administration**: Secure remote server management
- **File Transfer**: Secure file copying with SCP and SFTP
- **Development**: Remote code editing and deployment
- **Database Access**: Secure tunneling to database servers
- **Web Development**: Secure access to web servers and applications

**SSH vs Other Protocols:**
- **vs TELNET**: Encrypted vs plain text communication
- **vs RDP**: Command-line focused vs graphical remote desktop
- **vs VPN**: Application-specific vs network-wide encryption
- **vs FTP**: Secure file transfer (SFTP) vs insecure file transfer

## VPN

### Key Points

**Purpose:**
- VPN (Virtual Private Network) creates secure connections over public Internet infrastructure
- Enables remote access to private networks as if physically present on-site
- Provides economical solution for connecting geographically distributed offices
- Essential for secure remote work and private network access

**VPN Acronym Breakdown:**
- **V (Virtual)**: Uses existing Internet infrastructure instead of dedicated private lines
- **P (Private)**: Encrypts traffic to ensure confidentiality and data protection
- **N (Network)**: Creates secure network tunnel for data transmission

**Security Problem Solved:**
- Internet protocols (TCP/IP) focus on packet delivery, not security
- No built-in mechanisms to protect data from disclosure or alteration
- VPN adds encryption layer to secure data transmission over public networks
- Prevents eavesdropping and tampering with sensitive business communications

**VPN Use Cases:**

**Corporate Connectivity:**
- **Branch Offices**: Connect remote locations to main headquarters
- **Site-to-Site**: Permanent connections between company locations
- **Shared Resources**: Access to centralized servers, databases, and applications
- **Private Network Extension**: Remote sites appear as part of main network

**Remote Employee Access:**
- **Individual Users**: Single device connection to corporate network
- **Work from Home**: Secure access to company resources from anywhere
- **Mobile Workers**: Laptop and mobile device connectivity while traveling
- **Contractor Access**: Temporary secure access for external personnel

**VPN Architecture:**

**Site-to-Site VPN:**
- **VPN Server**: Located at main branch/headquarters
- **VPN Client**: Installed at remote branch locations
- **Encrypted Tunnel**: Secure connection between sites over Internet
- **Transparent Access**: Remote devices access main network resources seamlessly

**Remote Access VPN:**
- **Individual Connection**: Single device connects to VPN server
- **Client Software**: VPN application installed on user's device
- **Authentication**: Username/password or certificate-based login
- **Traffic Routing**: All or selected traffic routed through VPN tunnel

**VPN Traffic Flow:**

**Encrypted Traffic Path:**
- **Client to VPN Server**: All traffic encrypted and transmitted through tunnel
- **VPN Server Processing**: Decrypts traffic and forwards to destination
- **Response Path**: Return traffic encrypted and sent back through tunnel
- **End-to-End Security**: Data protected throughout transmission

**Network Segmentation:**
- **VPN Tunnel**: Encrypted connection shown as secure blue lines
- **Internal Network**: Decrypted traffic within private network (green lines)
- **Internet Transit**: Only encrypted data visible to ISPs and intermediaries
- **Local Decryption**: Data decrypted only at VPN endpoints

**IP Address Masking:**

**Public IP Hiding:**
- **Apparent Location**: Internet services see VPN server's IP address
- **Geographic Spoofing**: Appear to be located where VPN server resides
- **ISP Traffic Hiding**: Local ISP only sees encrypted VPN traffic
- **Censorship Circumvention**: Bypass geographical restrictions and blocking

**Location Example:**
- **Japan VPN Server**: Connecting to Japanese VPN server
- **Local Service Adaptation**: Websites display Japanese language/content
- **Regional Access**: Access to Japan-specific services and content
- **IP Geolocation**: Services identify user as located in Japan

**VPN Security Features:**

**Encryption Protocols:**
- **IPSec**: Internet Protocol Security for site-to-site connections
- **OpenVPN**: Open-source SSL/TLS-based VPN protocol
- **WireGuard**: Modern, lightweight VPN protocol with strong cryptography
- **SSTP**: Secure Socket Tunneling Protocol using SSL/TLS

**Authentication Methods:**
- **Username/Password**: Traditional credential-based authentication
- **Digital Certificates**: PKI-based authentication for enhanced security
- **Two-Factor Authentication**: Additional security layer with tokens/apps
- **Pre-Shared Keys**: Shared secret keys for site-to-site connections

**VPN Deployment Models:**

**Full Tunnel VPN:**
- **All Traffic Routing**: Complete Internet traffic routed through VPN
- **Maximum Security**: All communications protected and encrypted
- **IP Address Masking**: Complete hiding of user's actual location
- **Performance Impact**: All traffic subject to VPN server processing

**Split Tunnel VPN:**
- **Selective Routing**: Only specific traffic routed through VPN
- **Private Network Access**: VPN used only for corporate resources
- **Direct Internet Access**: Non-corporate traffic uses local connection
- **Performance Optimization**: Reduced load on VPN server

**VPN Security Considerations:**

**DNS Leak Protection:**
- **DNS Query Routing**: Ensure DNS requests go through VPN tunnel
- **Leak Testing**: Verify actual IP address isn't exposed
- **DNS Server Configuration**: Use VPN provider's DNS servers
- **WebRTC Blocking**: Prevent browser protocols from revealing real IP

**VPN Provider Trust:**
- **Logging Policies**: Choose providers with no-logs policies
- **Jurisdiction**: Consider legal jurisdiction of VPN provider
- **Encryption Standards**: Verify strong encryption implementations
- **Kill Switch**: Automatic disconnection if VPN connection fails

**Common VPN Protocols:**

**IPSec (Internet Protocol Security):**
- **Site-to-Site Standard**: Most common for business connections
- **Strong Security**: Military-grade encryption and authentication
- **Complex Setup**: Requires detailed configuration and management
- **Router Integration**: Built into many enterprise networking equipment

**OpenVPN:**
- **SSL/TLS Based**: Uses proven encryption technologies
- **Cross-Platform**: Available for all major operating systems
- **Open Source**: Transparent code available for security auditing
- **Flexible Configuration**: Supports various deployment scenarios

**Legal and Compliance:**

**Regulatory Considerations:**
- **Country Laws**: Some nations prohibit or restrict VPN usage
- **Travel Restrictions**: VPN laws may vary by destination country
- **Corporate Policies**: Company guidelines for VPN usage
- **Compliance Requirements**: Industry regulations affecting VPN deployment

**Best Practices:**
- **Legal Research**: Check local laws before VPN deployment
- **Company Policies**: Establish clear VPN usage guidelines
- **Security Auditing**: Regular testing of VPN configurations
- **User Training**: Educate users on proper VPN usage and security

**VPN Performance Factors:**

**Latency Considerations:**
- **Geographic Distance**: Distance to VPN server affects response time
- **Server Load**: Overloaded servers cause performance degradation
- **Encryption Overhead**: Cryptographic processing adds latency
- **Internet Path**: Quality of Internet connection to VPN server

**Bandwidth Management:**
- **Encryption Impact**: Security processing may reduce throughput
- **Server Capacity**: VPN server bandwidth limits affect speed
- **ISP Throttling**: Some ISPs may limit VPN traffic
- **Protocol Efficiency**: Different VPN protocols have varying overhead

## Encapsulation

### Key Points

**Encapsulation Process:**
Each layer adds headers (and sometimes trailers) to data received from the layer above, then passes it down to the layer below.

**Data Units by Layer:**

| Layer | Data Unit Name | What Gets Added |
|-------|----------------|-----------------|
| Application Layer | Application Data | Original user data (email, message, etc.) |
| Transport Layer | TCP Segment / UDP Datagram | TCP or UDP header |
| Network Layer | IP Packet | IP header (source & destination IP) |
| Data Link Layer | Frame | Ethernet/WiFi header and trailer |

**Encapsulation Flow:**
1. **Application Data** → User input (email, web request, etc.)
2. **TCP Segment/UDP Datagram** → Transport layer adds TCP/UDP header
3. **IP Packet** → Network layer adds IP header
4. **Frame** → Data link layer adds Ethernet/WiFi header and trailer

**Key Terms:**
- **TCP Segment**: Data unit at transport layer when using TCP
- **UDP Datagram**: Data unit at transport layer when using UDP
- **IP Packet**: Data unit at network layer
- **Frame**: Data unit at data link layer (WiFi/Ethernet)

**Process is Reversed:**
On the receiving end, each layer removes its header/trailer and passes data up to the layer above until application data is extracted.

## DHCP

### Key Points

**Purpose:**
- Automatically configures network settings for devices
- Eliminates manual configuration and prevents IP address conflicts
- Essential for mobile devices connecting to different networks

**Required Network Settings:**
- IP address and subnet mask
- Default gateway (router)
- DNS server address

**DHCP Details:**
- Application layer protocol using UDP
- Server listens on UDP port 67
- Client sends from UDP port 68

**DORA Process (Discover, Offer, Request, Acknowledge):**
1. **DHCP Discover**: Client broadcasts DHCPDISCOVER to find DHCP server
2. **DHCP Offer**: Server responds with DHCPOFFER containing available IP address
3. **DHCP Request**: Client sends DHCPREQUEST to accept the offered IP
4. **DHCP Acknowledge**: Server confirms with DHCPACK that IP is assigned

**Key Observations:**
- Client starts with only MAC address (no IP configuration)
- Discover and Request packets sent from 0.0.0.0 to broadcast 255.255.255.255
- Client uses broadcast MAC address (ff:ff:ff:ff:ff:ff) for initial packets
- Server provides IP lease, gateway, and DNS server information

## ARP

### Key Points

**Purpose:**
- Address Resolution Protocol (ARP) resolves IP addresses to MAC addresses
- Enables communication between devices on the same Ethernet/WiFi network
- Bridges Layer 3 (IP) addressing to Layer 2 (MAC) addressing

**MAC Address Details:**
- 48-bit number represented in hexadecimal notation
- Example: 7C:DF:A1:D3:8C:5C or 44:DF:65:D8:FE:6C
- Required for direct communication on Ethernet/WiFi networks

**Ethernet Frame Header Contains:**
- Destination MAC address
- Source MAC address  
- Type field (e.g., IPv4)

**ARP Process:**
1. **ARP Request**: Host broadcasts "Who has [IP]? Tell [my IP]"
   - Sent to broadcast MAC address (ff:ff:ff:ff:ff:ff)
   - Contains requester's MAC and IP addresses
2. **ARP Reply**: Target host responds with "IP is at [MAC address]"
   - Sent directly to requester's MAC address
   - Contains target's MAC address

**Key Technical Details:**
- ARP packets are encapsulated directly in Ethernet frames (not in IP packets)
- Operates at Layer 2 (deals with MAC addresses) but supports Layer 3 (IP operations)
- Only needed when devices communicate on the same network segment
- ARP cache stores IP-to-MAC mappings to avoid repeated requests

## ICMP

### Key Points

**Purpose:**
- Internet Control Message Protocol (ICMP) used for network diagnostics and error reporting
- Essential for network troubleshooting and monitoring
- Operates at Layer 3 (Network Layer)

**Common ICMP Tools:**

**Ping Command:**
- Uses ICMP Echo Request (Type 8) and Echo Reply (Type 0)
- Tests connectivity to target system
- Measures round-trip time (RTT)
- Shows if target is alive and reachable

**Ping Output Information:**
- Packet loss percentage
- RTT statistics: min/avg/max/standard deviation
- TTL (Time-to-Live) values
- Sequence numbers for tracking packets

**Traceroute Command:**
- Called `traceroute` on Linux/Unix, `tracert` on Windows
- Discovers route from source to destination
- Uses TTL field manipulation to reveal intermediate routers

**How Traceroute Works:**
- Sends packets with incrementing TTL values (1, 2, 3, etc.)
- When TTL reaches 0, router drops packet and sends ICMP Time Exceeded (Type 11)
- Each router in path reveals itself through Time Exceeded messages
- Final destination responds with Echo Reply

**Limitations:**
- Firewalls may block ICMP packets
- Some routers don't respond to ICMP queries
- Routes may change between traceroute runs
- Private IP addresses may be revealed in ISP networks

## Routing

### Key Points

**Purpose:**
- Routing determines the best path for data packets to travel between networks
- Essential for connecting separate networks across the Internet
- Routers use routing algorithms to make forwarding decisions

**Routing Challenges:**
- Internet consists of millions of routers and billions of devices
- Multiple paths typically exist between source and destination
- Need algorithms to determine optimal routes
- Must handle network changes and failures

**Common Routing Protocols:**

**OSPF (Open Shortest Path First):**
- Link-state routing protocol
- Routers share network topology information
- Each router builds complete network map
- Calculates shortest paths to all destinations
- Used within autonomous systems (interior gateway protocol)

**EIGRP (Enhanced Interior Gateway Routing Protocol):**
- Cisco proprietary protocol
- Hybrid routing algorithm combining distance-vector and link-state features
- Considers multiple metrics (bandwidth, delay, reliability)
- Fast convergence and loop prevention
- Used within autonomous systems

**BGP (Border Gateway Protocol):**
- Primary routing protocol for the Internet
- Exterior gateway protocol connecting different autonomous systems
- Used by Internet Service Providers (ISPs)
- Policy-based routing with path attributes
- Ensures global Internet connectivity

**RIP (Routing Information Protocol):**
- Simple distance-vector protocol
- Uses hop count as metric (maximum 15 hops)
- Suitable for small networks
- Easy to configure but slow convergence
- Older protocol, less commonly used today

## NAT

### Key Points

**Purpose:**
- Network Address Translation (NAT) addresses IPv4 address space depletion
- Allows multiple private IP addresses to share one public IP address
- Enables Internet access for many devices using minimal public IPs

**Problem Solved:**
- IPv4 supports maximum ~4 billion devices (2^32 addresses)
- Exponential growth of Internet-connected devices (computers, phones, IoT)
- Need to conserve public IP addresses

**How NAT Works:**
- Router maintains translation table mapping internal to external addresses
- Internal network uses private IP addresses
- External network sees only the router's public IP address
- Router tracks connections by mapping internal IP:port to external IP:port

**NAT Translation Example:**
- **Internal**: Device 192.168.0.129:15401 connects to web server
- **External**: Web server sees connection from 212.3.4.5:19273
- **Router**: Maintains mapping between these address:port pairs

**Key Benefits:**
- Conserves public IP addresses (uses 1 instead of many)
- Provides basic security through address hiding
- Allows network restructuring without external changes
- Enables private addressing schemes

**Technical Details:**
- Router must track all active connections (unlike simple routing)
- Translation happens at both IP and port levels
- Maintains state information for ongoing sessions
- Private addresses on internal side, public on external side

**Limitations:**
- Can complicate certain protocols and applications
- May break peer-to-peer connections
- Introduces single point of failure
- Can impact network performance due to translation overhead

## DNS

### Key Points

**Purpose:**
- Domain Name System (DNS) maps domain names to IP addresses
- Eliminates need to memorize IP addresses for websites
- Operates at Layer 7 (Application Layer) of OSI model

**DNS Protocol Details:**
- Uses UDP port 53 by default (faster, connectionless)
- Uses TCP port 53 as fallback (reliable, for larger responses)
- Hierarchical distributed database system

**Common DNS Record Types:**

**A Record:**
- Maps hostname to IPv4 address(es)
- Example: example.com → 172.17.2.172
- Most common DNS record type

**AAAA Record:**
- Maps hostname to IPv6 address(es)
- "Quad-A" record (not battery size or AAA authentication)
- Example: example.com → 2606:2800:21f:cb07:6820:80da:af6b:8b2c

**CNAME Record:**
- Canonical Name record maps domain to another domain
- Creates aliases for domain names
- Example: www.example.com → example.com

**MX Record:**
- Mail Exchange record specifies mail server for domain
- Used when sending emails to @domain addresses
- Includes priority values for multiple mail servers

**DNS Query Process:**
1. Browser queries DNS server for domain's A record
2. DNS server responds with IP address
3. Browser connects to the returned IP address
4. For emails, mail server queries MX record instead

**DNS Tools:**
- **nslookup**: Command-line tool for DNS lookups
- Shows both IPv4 (A) and IPv6 (AAAA) addresses
- Displays authoritative vs non-authoritative answers

**Technical Details:**
- Single domain lookup can generate multiple DNS queries
- Separate queries for A and AAAA records
- DNS responses contain query ID for matching requests
- Caching improves performance and reduces server load

## WHOIS

### Key Points

**Purpose:**
- WHOIS provides information about domain name registrants
- Shows who has authority to set DNS records for a domain
- Publicly available database of domain registration information

**Domain Registration Process:**
- Anyone can register available domain names for one or more years
- Must pay annual registration fees
- Required to provide accurate contact information
- Registrant gains authority to set DNS records (A, AAAA, MX, etc.)

**WHOIS Record Information:**
- **Registrant Details**: Name, organization, address, phone, email
- **Registration Dates**: Creation date, last updated, expiration date
- **Registrar Information**: Company that processed the registration
- **Name Servers**: DNS servers authoritative for the domain
- **Administrative/Technical Contacts**: People responsible for domain management

**Privacy Protection:**
- Privacy services can hide registrant information from public WHOIS records
- Shows privacy service details instead of actual registrant information
- Example: "Registration Private" or "Domains By Proxy, LLC"
- Protects personal information while maintaining legal requirements

**WHOIS Tools:**
- **Online Services**: Web-based WHOIS lookup tools
- **Command Line**: `whois` command available on Linux systems
- **Registrar Portals**: Direct access through domain registrar websites

**Key Information Fields:**
- Domain Name and Registry Domain ID
- Registrar WHOIS Server and URL
- Creation, Updated, and Expiration dates
- Registrar name and IANA ID
- Abuse contact information
- Registrant, Administrative, and Technical contact details

**Use Cases:**
- Domain ownership verification
- Contact information for technical issues
- Due diligence for domain purchases
- Investigating suspicious domains
- Legal and compliance research

## FTP

### Key Points

**Purpose:**
- File Transfer Protocol (FTP) designed specifically for efficient file transfer
- More efficient than HTTP for file transfer operations
- Can achieve higher speeds than HTTP under equal conditions
- Operates at Layer 7 (Application Layer)

**FTP Protocol Details:**
- Server listens on TCP port 21 by default (control connection)
- Uses separate data connection for actual file transfers
- Two-connection architecture: control and data channels

**Common FTP Commands:**
- **USER**: Input username for authentication
- **PASS**: Enter password for authentication
- **RETR**: Retrieve (download) file from server to client
- **STOR**: Store (upload) file from client to server
- **LIST**: List directory contents (sent when client types `ls`)
- **TYPE**: Switch transfer mode (ASCII or Binary)

**FTP Transfer Modes:**
- **ASCII Mode**: For text files (handles line ending conversions)
- **Binary Mode**: For binary files (exact byte-for-byte transfer)
- Default is usually binary mode

**FTP Connection Types:**
- **Control Connection**: Commands and responses (port 21)
- **Data Connection**: File transfers and directory listings
- Each file transfer uses a separate data connection

**Anonymous FTP:**
- Common configuration allowing public access
- Username: "anonymous"
- Password: Often not required or uses email address
- Provides read-only access to public files

**FTP Session Example:**
1. Connect to FTP server on port 21
2. Authenticate with USER and PASS commands
3. Navigate directories and list files with ls/LIST
4. Set transfer mode with TYPE command
5. Download files with get/RETR command
6. Disconnect with quit command

**Security Considerations:**
- FTP transmits credentials and data in plain text
- Vulnerable to eavesdropping and interception
- SFTP (SSH File Transfer Protocol) or FTPS (FTP over SSL/TLS) provide secure alternatives
- Anonymous FTP should be carefully configured to prevent unauthorized access

**Key Features:**
- Efficient for large file transfers
- Supports resume/restart of interrupted transfers
- Directory navigation and file management
- Both active and passive connection modes

## SMTP

### Key Points

**Purpose:**
- Simple Mail Transfer Protocol (SMTP) defines how email is sent
- Handles communication between mail client and mail server
- Facilitates server-to-server email transfer
- Operates at Layer 7 (Application Layer)

**SMTP Protocol Details:**
- Server listens on TCP port 25 by default
- Text-based protocol using human-readable commands
- Uses single persistent connection for entire email session
- Similar to postal service analogy (greeting, addressing, sending package)

**Essential SMTP Commands:**
- **HELO/EHLO**: Initiates SMTP session and identifies client
- **MAIL FROM**: Specifies sender's email address
- **RCPT TO**: Specifies recipient's email address
- **DATA**: Indicates start of email message content
- **.** (dot): Sent alone on line to indicate end of message
- **QUIT**: Terminates SMTP session

**SMTP Session Flow:**
1. Client connects to SMTP server on port 25
2. Server responds with greeting (220 code)
3. Client sends HELO/EHLO with identification
4. Client specifies sender with MAIL FROM
5. Client specifies recipient(s) with RCPT TO
6. Client sends DATA command to begin message
7. Client sends email headers and body
8. Client sends single dot (.) to end message
9. Client sends QUIT to close session

**Email Message Structure:**
- **Headers**: From, To, Subject, Date, etc.
- **Blank Line**: Separates headers from body
- **Message Body**: Actual email content
- **End Marker**: Single dot on its own line

**SMTP Response Codes:**
- **220**: Service ready
- **250**: Requested action completed successfully
- **354**: Start mail input (after DATA command)
- **221**: Service closing connection
- **5xx**: Permanent errors

**Security Considerations:**
- SMTP transmits emails in plain text by default
- Vulnerable to eavesdropping and interception
- SMTP Authentication (SMTP AUTH) adds password protection
- SMTPS (SMTP over SSL/TLS) provides encryption
- Modern implementations use STARTTLS for security

**Related Protocols:**
- **POP3**: Post Office Protocol for retrieving emails
- **IMAP**: Internet Message Access Protocol for email management
- **SMTPS**: Secure SMTP with SSL/TLS encryption

**Testing SMTP:**
- Can use telnet to manually send emails for testing
- Helps understand underlying protocol mechanics
- Useful for troubleshooting email delivery issues
- Shows exact commands email clients use

## POP3

### Key Points

**Purpose:**
- Post Office Protocol version 3 (POP3) enables email retrieval from mail servers
- Allows mail clients to download emails to local storage
- Complements SMTP (sending) with email retrieval functionality
- Operates at Layer 7 (Application Layer)

**Email System Analogy:**
- **SMTP**: Like handing mail to post office for delivery
- **POP3**: Like checking your personal mailbox for new mail
- SMTP handles outgoing mail, POP3 handles incoming mail retrieval

**POP3 Protocol Details:**
- Server listens on TCP port 110 by default
- Text-based protocol with simple command structure
- Requires authentication before accessing mailbox
- Downloads emails to client (typically removes from server)

**Common POP3 Commands:**
- **USER <username>**: Identifies the user account
- **PASS <password>**: Provides user's password for authentication
- **STAT**: Requests number of messages and total mailbox size
- **LIST**: Lists all messages with their sizes
- **RETR <message_number>**: Retrieves specific message content
- **DELE <message_number>**: Marks message for deletion
- **QUIT**: Ends POP3 session and applies changes (deletions)

**POP3 Session Flow:**
1. Client connects to POP3 server on port 110
2. Server responds with greeting (+OK message)
3. Client authenticates with USER and PASS commands
4. Client checks mailbox status with STAT
5. Client lists messages with LIST command
6. Client retrieves specific messages with RETR
7. Client optionally deletes messages with DELE
8. Client ends session with QUIT

**POP3 Response Format:**
- **+OK**: Positive response (command successful)
- **-ERR**: Negative response (command failed or error)
- **Message Content**: Follows successful RETR command
- **End Marker**: Single dot (.) on its own line

**Email Message Structure Retrieved:**
- **Headers**: Return-path, Envelope-to, Delivery-date, Received, From, To, Subject
- **Blank Line**: Separates headers from message body
- **Message Body**: Actual email content
- **End Marker**: Single dot on separate line

**Security Vulnerabilities:**
- POP3 transmits credentials and email content in plain text
- Passwords and messages visible to network packet capture
- Vulnerable to eavesdropping and man-in-the-middle attacks
- POP3S (POP3 over SSL/TLS) provides encrypted alternative

**POP3 vs IMAP:**
- **POP3**: Downloads emails to client, typically removes from server
- **IMAP**: Keeps emails on server, allows multiple device access
- **POP3**: Better for single-device access and limited server storage
- **IMAP**: Better for multi-device access and server-side management

**Testing POP3:**
- Can use telnet to manually retrieve emails for testing
- Useful for troubleshooting email retrieval issues
- Shows exact protocol exchange between client and server
- Demonstrates security vulnerabilities of plain text transmission

## IMAP

### Key Points

**Purpose:**
- Internet Message Access Protocol (IMAP) enables synchronized email access across multiple devices
- Maintains centralized email storage on server with client synchronization
- Superior to POP3 for multi-device email access scenarios
- Operates at Layer 7 (Application Layer)

**IMAP vs POP3 Key Differences:**
- **IMAP**: Keeps emails on server, synchronizes across all clients
- **POP3**: Downloads emails to single client, typically removes from server
- **IMAP**: Better for multiple devices (desktop, laptop, smartphone)
- **POP3**: Better for single device with limited server storage

**IMAP Protocol Details:**
- Server listens on TCP port 143 by default
- More complex command structure than POP3
- Supports mailbox folders and message management
- Synchronizes read, moved, and deleted message states

**Common IMAP Commands:**
- **LOGIN <username> <password>**: Authenticates user with credentials
- **SELECT <mailbox>**: Selects specific mailbox folder to work with
- **FETCH <mail_number> <data_item>**: Retrieves message content
  - Example: `FETCH 3 body[]` retrieves message 3 with headers and body
- **MOVE <sequence_set> <mailbox>**: Moves messages to different mailbox
- **COPY <sequence_set> <mailbox>**: Copies messages to another mailbox
- **LOGOUT**: Ends IMAP session and logs out

**IMAP Session Flow:**
1. Client connects to IMAP server on port 143
2. Server responds with capability greeting
3. Client authenticates with LOGIN command
4. Client selects mailbox with SELECT command
5. Server responds with mailbox status (message count, flags, etc.)
6. Client fetches specific messages with FETCH command
7. Client performs message operations (move, copy, delete)
8. Client logs out with LOGOUT command

**IMAP Response Elements:**
- **Server Capabilities**: Supported features and extensions
- **Mailbox Status**: Message count, recent messages, flags
- **Message Flags**: \Answered, \Flagged, \Deleted, \Seen, \Draft
- **UID Information**: Unique identifiers for message tracking

**IMAP Advantages:**
- **Multi-device synchronization**: Access same mailbox from any device
- **Server-side storage**: Emails remain on server for backup/access
- **Folder management**: Organize emails in server-side folders
- **Partial message retrieval**: Download headers first, bodies on demand
- **Search capabilities**: Server-side email searching

**IMAP Disadvantages:**
- **Storage requirements**: Uses more server storage than POP3
- **Network dependency**: Requires connection to access emails
- **Bandwidth usage**: May use more bandwidth for synchronization
- **Complexity**: More complex protocol than POP3

**Security Considerations:**
- IMAP transmits credentials and email content in plain text by default
- Vulnerable to network eavesdropping and packet capture
- IMAPS (IMAP over SSL/TLS) provides encrypted alternative
- Uses port 993 for secure IMAP connections

**Testing IMAP:**
- Can use telnet to manually access IMAP servers for testing
- Useful for troubleshooting email synchronization issues
- Shows detailed protocol exchange and server capabilities
- Demonstrates command structure and response format

