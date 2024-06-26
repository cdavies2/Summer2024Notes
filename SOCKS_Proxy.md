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
    *   
# Software
## Servers
