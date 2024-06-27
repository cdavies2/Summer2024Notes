# SOCKS
* SOCKS is an Internet protocol that exchanges network packets between a client and server through a proxy server
* SOCKS5 optionally provides authenticatin so only authorized users may access a server
* A SOCKS server proxies TCP connections to an arbitrary IP address, and provides a means for UDP packets to be forwarded.
## Acronym
* SOCKS is sometimes defined as an acronym for "socket secure"
* Originally though, SOCKS referred to a specific proxy protocol designed to facilitate communication between clients and servers through a firewall.
## Usage
* SOCKS is a de facto standard for circuit-level gateways. It can be used as...
  * A circumvention tool, allowing traffic to bypass Internet filtering to access content otherwise blocked (EX: by governments, workplaces, schools, and country-specific web services). Because SOCKS is very detectable, a common approach is to present a SOCKS interface for more sophisticated protocols (EX: the Tor onion proxy software)
  * Prociding similar functionality to a VPN, allowing connections to be forwarded to a server's "local" network.

# Protocol
* A typical SOCKS4 connection request looks like this
                         VER      CMD      DSTPORT      DSTIP      ID
       Byte Count        1        1        2            4          Variable
*  VER- SOCKS version number, 0x04 for this version
*  CMD-command code
    * 0x01=establish a TCP/IP stream connection
    * 0x02=establish a TCP/IP port binding
* DSTPORT-2 byte port number (in network byte order)
* DESTIP-IPv4 Address, 4 bytes (in network byte order)
* ID-the user ID string, variable length, null-terminated

  ## Response Packet from Server
                        VN        REP      DSTPORT     DSTIP
        Byte Count      1        1        2            4
*   VN-reply version, null byte
*   REP-reply code
*   DSTPORT-destination port, meaningful if granted in BIND, otherwise ignore
*   DSTIP-destination IP, as above, the ip:port the client should bind to
*   The "bind" command allows incoming connections for protocols such as active FTP
## Reply Code Meaning
* 0x5A-request granted
* 0x5B-request rejected or failed
* 0x5C-request failed because client is not running identd (or not reachable from server)
* 0x5D-request failed because client's identd couldn't confirm the user ID in the request

# SOCKS5
* SOCKS5 is an incompatible extension of the SOCKS4 protocol which offers more choices for authentication and adds support for IPV6 and UDP, the latter of which is used for DNS lookups. The initial handshake consists of...
    * Client connects and sends a greeting, which includes a list of authentication methods supported.
    * Server chooses one of the methods (or sends a failure response)
    * Messages can now pass between the client and server
    * Client sends a connection request similar to SOCKS4
    * Server responds similar to SOCKS4
## Client Greeting
                    VER        NAUTH    AUTH
     Byte Count      1          1        Variable
*   Ver-SOCKS version (0x05)
*   NAUTH-number of authentication methods supported, uint8
*   AUTH-Authentication methods, which are numbered as follows:
    *   0x00: No authentication
    *   0x01: GSSAPI (used to allow programs to access security services)
    *   0x02: Username/password
    *   0x03-0x7f: methods assigned by IANA
      *   0x03: Challenge-Handshake Authentication Protocol
      *   0x04: Unassigned
      *   0x05: Challenge-Response Authentication Method
      *   0x06: Secure Sockets Layer
      *   0x07: NDS Authentication
      *   0x08: Multi-Authentication Framework
      *   0x09: JSON Parameter Block
      *   0x0A-0x7F: Unassigned
    * 0x80-0xFE: Methods reserved for private use
 ## Server Choice
                     VER        CAUTH
     Byte Count      1          1
    * VER- SOCKS version (0x05)
    * CAUTH-chosen authentication method, or 0xFF if no acceptable methods were offered
 ## Client Authentication Request, 0x02
                         VER      IDLEN    ID              PWLEN      PW
       Byte Count        1        1        (1-255)         4          (1-255)
 * VER-0x01 for current version of username/password authentication
 * IDLEN, ID-username length, uint8, username as bytestring
 * PWLEN, PW-password length, uint8, password as bytestring
## Server Response, 0x02
                     VER        STATUS
     Byte Count      1          1
 * VER-0x01 for current version of username/password authentication
 * STATUS-0x00 success, otherwise failure, connection must be closed
 ## Client Connection Request
                         VER      CMD      RSV      DSTADDR    DSTPORT
       Byte Count        1        1        1        Variable   2
 * VER-SOCKS version (0x05)
 * CMD-command code:
   * 0x01: establish a TCP/IP stream connection
   * 0x02: establish a TCP/IP port binding
   * 0x03: associate a UDP port
 * RSV-reserved, must be 0x00
 * DSTADDR-destination address, see the address structure above
 * DSTPORT-port number in a network byte order
## Response Packet from Server
                         VER      STATUS   RSV      BNDADDR    BNDPORT
       Byte Count        1        1        1        Variable   2
 * VER-SOCKS version (0x05)
 * STATUS-status code
   * 0x00: request granted
   * 0x01: general failure
   * 0x02: connection not allowed by ruleset
   * 0x03: network unreachable
   * 0x04: host unreachable
   * 0x05: connection refused by destination host
   * 0x06: TTL expired
   * 0x07: command not supported/protocol error
   * 0x08: address type not supported
 *  RSV-reserved, must be 0x00
 *  BNDADDR-server bound address in the "SOCKS5 address" format
 *  BNDPORT-served bound port number in a network byte order
 *  A convention from cURL exists to label the domain name variant of SOCKS5 "socks5h", and the other simply "socks5"
# Software 
## Servers
* Sun Java System Web Proxy Server-caching proxy server running on Solaris, Linux and Windows servers that support HTTPS, NSAPI I/O filters, dynamic reconfiguration, SOCKSv5 and reverse proxy
* WinGate-multi protocol proxy server and SOCKS server for Microsoft Windows which supports SOCKS4 and SOCKS5. It also supports handing over SOCKS connections to the HTTP proxy, so can cache and scan HTTP over SOCKS.
* Dante-circuit level SOCKS server that can be used to provide convenient and secure network connectivity,requiring only the host Dante runs on to have external network connectivity.
* Socksgate5-application-SOCKS firewall with inspection feature on Layer 7 of the OSI model, the Application Layer. Because packets are inspected at 7 OSI Level the application-SOCKS firewall may search for protocol non-compliance and blocking specified content.
* HevSocks5Server-high performance and low-overhead SOCKS server for Unix (Linux/BSD/ macOS). It supports standard TCP-CONNECT and UDP-ASSOCIATE methods and multiple username/password authentication.
* OpenSSH-allows dynamic creation of tunnles, specified via a subset of the SOCKS protocol, supporting the CONNECT command.
* PuTTY-Win32 SSH client that supports local creation of SOCKS (dynamic) tunnels through remote SSH servers.
* ShimmerCat-web server that uses SOCKS5 to simulate an internal network, allowing web developers to test their local sites without modifying their /etc/hosts file
* Tor-system intended to enable online anonymity. Tor offers a TCP-only SOCKS server interface to its clients.
## Clients
* Client software must have native SOCKS support in order to connect through SOCKS
### Browser
* Chrome: support SOCKS4, SOCKS4a, and SOCKS5
* FireFox: support SOCKS4, SOCKS4a, and SOCKS5
* Internet Explorer and EdgeHTML-based Microsoft Edge: support SOCKS4 only
* Chromium-based Microsoft Edge: support SOCKS4, SOCKS4a and SOCKS5.
### Proxies
* Win2Socks-enables applications to access the network through SOCKS5, HTTPS or Shadowsocks
* proxychains-a Unix program that forces TCP traffic through SOCKS or HTTP proxies on (dynamically-linked) programs it launches. Works on various Unix-like systems.
* Privoxy-non caching SOCKS-to-HTTP proxy
* Tinyproxy-a light weight HTTP/HTTPS proxy daemon for POSIX operating systems.
