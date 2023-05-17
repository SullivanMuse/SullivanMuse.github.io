---
title: "Networking"
date: 2023-05-16T17:45:01-05:00
draft: false
---

## General

- What is the Internet?
    - A network of billions of computing devices, called hosts
        - Each has a unique numeric IP (Internet Protocol) address
    - Many communication links
        - Copper wire, fiber optic
    - ICANN
        - Assigns IP addresses and DNS top-level domains
        - Operates 13 root DNS servers
    - IETF
        - Develops protocls that define how internet devices interact
        - RFCs (Request for comments) are its documents defining internet standards
- Application level - HTTPS, Email, SSH
- Network layer - IP
- Transport layer - TCP
- Link layer - WiFi
- Physical layer - UDP
- Hosts and routers
- Packets are an alternative to switching
- Throughput (bandwidth)
- Latency
- Circuit switching and packet switching can coexist within a single connection. Ex. Rocket delivers telemetry to ground using a reserved frequency connection to the ground. Ground station delivers telemetry home using internet packet switching.
- Internet delivers packets with "best effort" (it may not arrive.)

- Link layer (point-to-point connections that are bi-directional, and allow 0s and 1s to be transmitted (or idle))
    - Has propogation delay
        - Link latency
    - Operates at a certain constant bitrate

- Distributed routing algorithms are used to determine which address ranges are most quickly reachable
- Packet forwarding is the direction of packets according to rules
- Nodal delay (time for each hop)
    - dnodal = dproc + dqueue + dtrans + dprop
        - dnodal
            - check bit errors
            - choose output link
            - typically < msec
        - dqueue
            - time waiting for transmission
            - depends on congestion level of the outbound link
        - dtrans
            - packet size (bits) / link bandwidth (bps)
        - dprop
            - physical length of link / propogation speed
- ISPs have tiers
    - Tier 1 are in the business of direct global connection to other ISPs
    - Tier 2 are in the business of regional connections (large city, area of state)
    - Tier 3 (access ISPs) are in the business of selling access to individual building owners and residents
- Network layers in WWW
    - HTTP(S)
    - TLS
    - TCP
    - IP
    - Ethernet/Wifi

- Each computer has a unique IP address, but OS manages ports to separate packets meant for each process. This is handled by the TCP or UDP protocol.
    - Conventional port numbers
        - HTTP 80
        - HTTPS (HTTP + TLS encryption) 443
            - Override by appending port number to URL
        - SMTP (email) 25
        - DNS 53
- Multiple web tabs on same machine may use same destination port, but the combination of the source ip and port, and the destination ip and port (socket) is unique amongst the tabs
    - Sockets are bound to particular port numbers
- Ports below 1024 are privileged and require root access to send or recv
- TCP creates the illusion of a reliable, bidirectional pipe of data

- Layers
    - Link - share a physical channel among several transmitters and receivers
    - Network - route from source to destination
    - Transport - Multiplexing, ordering, acknowledgement, pacing
    - Session security - encryption, authentication
    - Application - domain specific concerns

## Protocol layers

- Link (raw data transfer)
    - Bitstream between two hosts
    - Raw data throughput
    - Can be unreliable (dropping some bits)
    - Examples: Wifi, ethernet, radio
- Network (routing)
    - Network of hosts
    - Provides unique identification of hosts using IP addresses
    - Packets are sent over network between two hosts
    - DNS services provide human-readable names for servers through nameservers (domain -> IP)
- Transport (reliability)
    - UDP
        - Unreliable
        - Provides ports for disambiguating destination of packet for different applications on the same host
        - Provides data integrity checking with checksums
    - TCP
        - Provides ordering, acknowledgement, and pacing
- Application (very domain-specific)
    - More specifically
        - Session layer (maintains connections, control ports and sessions)
        - Presentation layer (encryption, data formatting)
        - Application layer (human-computer interaction)
    - Runs on a certain port
    - Has a TCP or UDP socket
    - Assumes reliable (or unreliable) bytestream between two applications running on different hosts
    - Can be stateful (SMTP) or stateless (HTTP)
    - Must implement security services (end-to-end encryption, authentication)

## Protocols

### HTTP

- Used to transport .html and related files.
- Built on top of TCP (reliable bytestream)
- Request
    - URL
    - method
    - optional headers
    - optional body
- Response
    - humand-readable header with *response code*
    - optional headers
    - optional body
- **Stateless**
    - each request is self-contained
    - meaning of requests is independent
- Protocol
    1. Client creates TCP socket, and server accepts the socket
    2. Client writes HTTP request to socket; starts listening for response
    3. Server notices new data on socket and starts reading request data
    4. Server eventually notices that it has received a full HTTP request
    5. Server generates appropriate response
    6. Server writes HTTP reponse on TCP socket
    7. Client reads and parses response data
- HTTP methods and responses
    - GET: request data
    - POST: post to server, and possibly get data back
    - PUT: create a new document on server
    - DELETE: delete a document
    - [HEAD](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD): like GET, but just return headers
- Reponse codes (in first line of response)
    - 200 OK: success
    - 301 Moved Permanently: redirects to another URL
    - 403 Forbidden: lack permission
    - 404 Not Found: URL is bad
    - 500 Internal Server Error
    - ... this is a non-exhaustive list
- Cookies
    - Responses may include **cookies** in the header
    ```
    Set-Cookie: cookie-id=asdfpounervi
    ```
    - Which is then (usually) sent back with **all future requests**
    ```
    Cookie: cookie-id=asdfpounervi
    ```
    - Cookies may be used to uniquely identify users, which is why some people have privacy concerns about them
    - Cookies do not mean that HTTP has state. HTTP is still stateless because requests and responses are self-contained. They do not depend on previous messages to be processed correctly.
- REST (REpresentational State Transfer) APIs
    - Alternative to serving static webpages
    - Use HTTP methods to implement more general resource retrieval
    - Sort of like RPC (remote procedure calls)
    - Usually the body of a REST call is JSON
- Header example

```
GET /doc/test.html HTTP/1.1     -- Request line
Host: www.test101.com              | Request headers
Accept: image/gif, image/jpeg, */* |
Accept-Language: en-us             |
Accept-Encoding: gzip, deflate     |
User-Agent: Mozilla/4.0            |
Content-Length: 35                 |
                                -- Header and body separated by blank line
bookId=12345&author=Tan+Ah+Teck -- Request message body
```

- Official HTTP specification is maintained by IETF. There are multiple versions, additions, and extensions to HTTP, but [here](https://datatracker.ietf.org/doc/html/rfc1945) is the first offical RFC (request for comment)

### SMTP

- Peer-to-peer (P2P) protocol for email servers to exchange users' messages
- User agents use different protocols to fetch emails (IMAP, POP3, webmail)
- **Stateful**
    - Specifies a series of exchanges between client and server

### DNS

- Domain-name service is a map from human-readable hostnames to IP addresses
- Based on centralized trust
- IP addresses that are close together are often close geographically
- Domain names have no necessarily geographic association (exceptions, like country TLDs (top-level-domains) .uk)
- Like a phone book (association from names to phone numbers)
- Scalability
    - Distribute the database
    - Cache DNS lookups for short period
    - Hierarchical
        - TLDs (.edu, .com, .org)
        - Domain names (northwestern.edu)
        - Subdomains (kellogg.northwestern.edu)
        - Each level of DNS server only stores IP addresses for its subdomains
        - Each level of domain has an authoritative *nameserver*. This is the single source of truth for this domain
            - Technically, these nameservers are often made physically redundant using *IP anycast*
        - Root DNS servers store the IP addresses for TLD servers (.com, .edu, .us)
            - There are currently 13 (as of 2023-05-17) [root DNS servers](https://www.iana.org/domains/root/servers) maintained by IANA (a department of ICANN)
        - Every time you use DNS, you are **trusting that ICANN and all subdomain servers you are using are not tricking you and are not incorrectly implemented**
- Procedure
    - Query iteratively, starting with the root server
        - Each server replies with an answer of the name of server to contact
            - "I don't know this name, but try asking X"
    - For performance reasons, ISPs operate DNS resolvers for their customers which implement lots of caching (**security: we are trusting the ISPs to give us the correct domains**)
    - DNS records have a TTL (time to live) indicating when they expire
        - TTL is typically 300 to 86400 seconds (5 minutes to 24 hours)
- DNS records
    - Name (x.com)
    - Value (usually IP address)
    - Type
        - A is domain name -> IP address
        - MX (mail server for this domain)
        - NS - list nameservers for a given domain
        - SOA - store information about who created the DNS records and how they should be cached
        - SRV - list server for a specified service (generalization of MX)
        - TXT - generic key-value store
            - DKIM store email signature public key
        - SPF - list valid outbound mail servers for this domain
        - PTR - store reverse DNS records, IP address -> hostname
        - CNAME (Canonical NAME) - list aliases (www.x.com -> x.com)
            - Procedure: recurse
    - TTL (time-to-live)
- Domain registrars sell domains
    - They are licensed by XXXX
    - They then notify TLD servers about NS record
- Dynamic nameservers may **give different answers to different clients**
    - A simple scheme will just cycle through different servers for each successive client to provide *load-balancing*
        - This works well for HTTP because it is stateless
    - Load-balancing nameservers can also monitor the health of servers to exclude overwhelmed servers
    - May also consider geography to minimize propogation latency
- `traceroute IP_ADDR` cmd line tool can be used to see which machines are used to route your request
- CDNs (content delivery networks) are distributed servers that cache HTTP responses for local clients
    - These are like dynamic nameservers, but they work at HTTP level instead of DNS
- Many internet outages are due to local nameserver failures; Google provides free public DNS at 8.8.8.8, and 8.8.4.4
- Captive portals (Wifi login pages for free Wifi eg) are implemented using DNS

### TCP

- Part of the **Transport layer**
- Provides
    - Reliable bytestreams between clients (socket)
    - Multiplexing
    - Abstracts limited packet size into unlimited message length (packets -> bytestreams)
    - Ordering
        - Data must be *packetized* by sender and *reassembled* by receiver
        - Reassembly is done in the proper *order* regardless of delivery order
    - Acknowledgement
        - Delivery of each packet is acknowledged, so lost packets can be *retransmitted*
        - Almost bulletproof reliability, otherwise failed connection
    - Pacing
        - Sender adjusts packet send rate so neither receiver nor network are overwhelmed
        - Avoid filling up queues and dropping packets
- Acknowledgement
    - ACK (positive acknowledgement) - "Ok", "Mm-hmm"
    - NACK (negative acknowledgement) - "What?", "Can you repeat that?"
- Simplest protocol is "stop-and-wait"
    - Do not send next packet until reception of ack for current packet
    - However, this means that RTT limits throughput, which is not ideal
    - Throughput (bps) = Packet size (bits) / RTT (s)
    - Example:
        - An ISP's fiber link from New Jersey to San Jose, CA has 1 Gbps link, 15 ms propogation delay, 1.5 kB packet size
        - RTT = 2 * (15 ms + 1.5 kB * 8 b/B / 1 Gbps) = 30.01 ms
        - RTT is dominated by 30 ms round-trip propogation delay
        - Effective throughput is just 1.5 kB * 8 b/B / 30.01 ms = 250 kbps, which is **4000 times slower than the link speed**
- Better procedures
    - The fix for stop-and-wait is *pipelining*; allowing multiple in-flight packets
        - This means we need buffering and reception tables
        - All in-flight packets must be cached by sending until ack
        - All out of order packets must be reordered by receiver
        - *Window size* is maximum number of in-flight packets
            - Finite to limit the data buffering required and to limit load on network
    - Go back N
        - Receiver sends cumulative ACK "I got everything up to seq number X"
        - [Beautiful interactive visualization](https://stevetarzia.com/340/gbn.html) of this protocol
    - Selective repeat
        - Receiver must now also keep buffer
        - Receiver sends ACK for each packet
        - Window length must be less than half the maximum sequence number

### UDP

- User datagram protocol
- Unreliable
- Does not add much to IP
    - Adds *port numbers* to packets, to distinguish between services on a machine
    - Adds *checksums* to verify that packet data was not corrupted

## Network architectures

### Client-server architecture

- Clients
    - Make requests
    - **Do not listen** for *unsolicited messages*
    - Do not communicate directly with other clients
- Servers
    - "Always" on
    - Permanent IP addresses
    - Usually have a DNS hostname
    - Usually reside in data centers
    - No display, keyboard, or mouse
    - **Listen** for requests from clients

### Peer-to-peer (P2P) architecture

- All participants have equal responsibilities
- Examples: napster, torrent networks
- Scalable
- Difficulties
    - Hosts join and leave the network (churn)
    - IP addresses change
    - Firewalls may block access to peers
    - Edge networks have limited upload speed
- Uses kind of centralized directory/tracker

## Appendix: Acronyms

| Acronym | Stands for | Means |
| --- | --- | --- |
| P2P | Peer-to-peer | Application architecture where all hosts on a network are equal, juxtaposed with client-server architecture.

## Cool links

- [Speedtest](fast.com)
- [Submarine Cables Map](https://www.submarinecablemap.com/)
- [Internet Exchange Point (IXP) Map](https://www.internetexchangemap.com/)

## References

Some of this post is remixed from a [lecture series](https://youtube.com/playlist?list=PLWl7jvxH18r3nnotitKkyAjq268PQGc0-) by [Steve Tarzia](https://www.youtube.com/@SteveTarzia) for the CS-340 Intro to Computing Networking course at Northwestern University.