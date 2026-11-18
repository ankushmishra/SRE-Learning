# Day 5 - Networking I: DNS, UDP, and TCP

## Session goal

Learn how DNS turns a domain name into IP addresses, how UDP and TCP differ,
and how to make a few read-only network checks from Ubuntu. By the end, capture
an IP address returned by `dig` and draw the TCP three-way handshake from
memory.

Suggested time: 90-120 minutes, including the exercises and notes.

## 1. Fundamentals

### 1.1 What happens when you look up a domain?

People use names such as `example.com`; computers use IP addresses to send
traffic. The Domain Name System (DNS) is a distributed naming system that lets a
client ask for records associated with a domain.

A typical lookup involves these parts:

1. The application asks the operating system for an address.
2. The system checks local sources such as its cache and hosts file.
3. If needed, a DNS resolver receives the query. It may already have a cached
   answer; otherwise, it finds the answer through the DNS hierarchy.
4. An authoritative DNS server returns the record for the domain.
5. The resolver returns the answer to the client and may cache it for the
   record's TTL (time to live).

The hierarchy includes root, top-level domain (such as `.com`), and
authoritative name servers. A recursive resolver performs the lookup on behalf
of a client. A DNS answer may contain more than one IP address, and results can
change as records or caches change.

Common record types include:

- `A` - an IPv4 address
- `AAAA` - an IPv6 address
- `CNAME` - an alias to another domain name
- `MX` - mail server information
- `NS` - authoritative name server information

### 1.2 DNS tools

- `dig` provides detailed DNS query results and is useful for troubleshooting.
- `nslookup` provides a simple interactive or command-line lookup.
- `getent hosts` checks name resolution through the system's configured name
  service path, which may include more than DNS.

DNS commonly uses UDP port 53 for ordinary queries. DNS can also use TCP port
53, including when a response is truncated and the client retries over TCP, or
for operations such as zone transfers. Do not assume that every DNS query uses
only UDP.

### 1.3 UDP and TCP

UDP (User Datagram Protocol) sends independent datagrams without first
establishing a connection. It has low protocol overhead, but does not guarantee
delivery, ordering, or retransmission. Applications that need those properties
must handle them themselves or use a protocol built on top of UDP.

TCP (Transmission Control Protocol) establishes a connection and provides an
ordered, reliable byte stream. It uses acknowledgements and retransmissions to
recover from loss. Those features add state and overhead, but are useful when
the application needs reliable ordered delivery.

| Property | UDP | TCP |
| --- | --- | --- |
| Connection setup | No transport handshake | Connection established before data |
| Delivery and order | Not guaranteed by UDP | Reliable and ordered byte stream |
| Typical fit | Real-time media, some DNS queries | Web pages, SSH, file transfer |

Voice and video applications often use UDP-based media because late packets can
be less useful than missing packets; the application can prioritize low delay
and handle loss or concealment itself. DNS uses both UDP and TCP depending on
the query and response.

### 1.4 The TCP three-way handshake

Before sending application data, a TCP client and server synchronize connection
state:

```text
Client                                      Server
  | -------- SYN (seq=x) --------------------> |
  | <--- SYN-ACK (seq=y, ack=x+1) ------------ |
  | -------- ACK (ack=y+1) ------------------> |
  |            connection established          |
```

- `SYN`: the client requests a connection and proposes an initial sequence
  number.
- `SYN-ACK`: the server acknowledges the client's sequence and proposes its
  own.
- `ACK`: the client acknowledges the server's sequence. Both sides can now
  exchange application data.

Practice drawing these three arrows and explaining what each acknowledges
without looking at the diagram.

### 1.5 A network troubleshooting view

Name resolution, reachability, and transport connections are separate checks:

1. Use `dig` or `nslookup` to see whether the name resolves.
2. Use `ping` to observe ICMP reachability and round-trip times when ICMP is
   permitted.
3. Use `ss` to inspect sockets and connections on your own machine.

A failed `ping` does not prove that a website or service is down; networks can
block ICMP while allowing application traffic. Similarly, a successful DNS
lookup does not prove that a service is reachable on its application port.

## 2. Practical lab

### 2.1 Prepare a workspace

Run these commands in the Ubuntu Terminal:

```bash
mkdir -p ~/sre-lab/day5
cd ~/sre-lab/day5
pwd
```

The final path should end in `/sre-lab/day5`. Use domains you commonly visit.
For the examples below, replace `example.com` with one of your chosen domains.

### 2.2 Query three domains with `dig`

Run a lookup for each of three domains you use:

```bash
dig example.com
dig example.org
dig example.net
```

To show only IPv4 answers:

```bash
dig +short A example.com
```

To query IPv6 records:

```bash
dig +short AAAA example.com
```

Record at least one IP address returned by `dig`. Answers can vary by time,
resolver, geographic location, and load-balancing setup. `+short` can return
multiple addresses or no address, depending on the record.

### 2.3 Compare with `nslookup`

Look up the same three domains with `nslookup`:

```bash
nslookup example.com
nslookup example.org
nslookup example.net
```

Observe the DNS server used and the returned address or addresses. The output
from `dig` and `nslookup` may differ in detail; both query DNS, but show results
in different formats.

### 2.4 Check reachability and round-trip time

Send four ICMP echo requests to each domain:

```bash
ping -c 4 example.com
ping -c 4 example.org
ping -c 4 example.net
```

For each, note whether replies arrived and the reported minimum, average, and
maximum round-trip time (RTT). Some hosts or networks block ICMP or rate-limit
replies. If there are no replies, record that result; do not conclude from it
alone that the host or its web service is down.

### 2.5 Inspect sockets and connections

Show TCP and UDP sockets, with numeric addresses and process details when
available:

```bash
ss -tunap
```

The options mean TCP, UDP, numeric addresses, all sockets, and process details.
Some process information may require elevated privileges. You can focus on TCP
connections with:

```bash
ss -tanp
```

If `netstat` is already installed, an alternative is:

```bash
netstat -tunap
```

`netstat` may not be installed by default. Use `ss` if it is unavailable; do
not install extra packages just for this exercise. These commands inspect
sockets on your machine and do not change network configuration.

### 2.6 Draw and recall the handshake

On paper, draw a client and server, then add these arrows in order:

1. Client to server: `SYN`
2. Server to client: `SYN-ACK`
3. Client to server: `ACK`

Hide the diagram above and recreate it from memory. Label what each side
acknowledges. Repeat until you can explain the exchange without notes.

## 3. Review and explain

Answer in your own words:

1. What problem does DNS solve?
2. What is the difference between a recursive resolver and an authoritative
   DNS server?
3. What information does an `A` record provide? What about an `AAAA` record?
4. Why might DNS use TCP as well as UDP?
5. What does TCP provide that UDP does not guarantee?
6. What are the three messages in the TCP handshake, and what does each do?
7. Why does a failed `ping` not necessarily mean a service is unavailable?
8. Which command showed the DNS answer, and which command showed local sockets?

## 4. Optional stretch: telecom analogy

Think about where name or service resolution happens in a mobile network you
know. DNS may resolve hostnames used by network services, while mobile network
procedures can also use identifiers, configured APN/DNN settings, and service
discovery mechanisms. These play related roles in finding a destination, but
they are not a one-to-one replacement for DNS. Write down one example from your
experience and identify what name or identifier is resolved, which system
performs the lookup, and what endpoint is returned.

## Day 5 notes

Record your observations:

- Domains tested:
  1.
  2.
  3.

- IP address returned by `dig` for one domain:
  - Domain:
  - Record type:
  - IP address:

- `nslookup` comparison:
  -

- Ping results and RTT observations:
  1.
  2.
  3.

- `ss` or `netstat` observation:
  -

- TCP handshake sketch completed from memory: Yes / Not yet

- Three things I learned:
  1.
  2.
  3.

- One command that was confusing and how I resolved it:
  -

- Troubleshooting observation:
  -

## Done when

- [ ] I queried three domains with `dig` and `nslookup`.
- [ ] I recorded an IP address returned by `dig` for one domain.
- [ ] I ran `ping` and recorded the results or noted that ICMP was blocked.
- [ ] I listed local sockets with `ss` or `netstat`.
- [ ] I can sketch and explain the TCP three-way handshake from memory.

