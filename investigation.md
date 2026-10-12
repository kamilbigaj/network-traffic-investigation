# Network Traffic Investigation Report

## 1. Investigation Scope

The purpose of this investigation was to analyse a packet capture using Wireshark and identify basic network communication patterns.

The investigation focused on:

- DNS resolution
- TCP connection establishment
- Client and server identification
- Source and destination ports
- TLS-protected communication

This report was created as part of the TryHackMe room [Wireshark: The Basics](https://tryhackme.com/room/wiresharkthebasics).

The analysed capture was named `exercise.pcapng`.

## 2. Environment

The investigation used:

- Wireshark
- A PCAPNG file provided by TryHackMe
- A Linux-based learning environment

## 3. DNS Analysis

The capture contained a DNS request for:

`contile.services.mozilla.com`

The DNS communication involved:

- Client: `10.10.57.178`
- DNS server: `10.0.0.2`
- Resolved IP address: `34.117.237.239`

The DNS response associated the domain `contile.services.mozilla.com` with the IP address `34.117.237.239`.

This resolution was then correlated with a later TCP connection to the same IP address.

## 4. Client and Server

Based on the observed traffic:

| Role | IP address |
|---|---|
| Client | `10.10.57.178` |
| Server | `34.117.237.239` |
| DNS server | `10.0.0.2` |

The client initiated the TCP connection by sending the first SYN packet.

## 5. TCP Three-Way Handshake

The TCP connection was established using the standard three-way handshake:

- SYN — packet 16873
- SYN/ACK — packet 16874
- ACK — packet 16875

The sequence shows that:

- The client initiated the connection
- The server responded
- The client acknowledged the response
- The TCP connection was successfully established

## 6. Ports

The observed connection used:

| Field | Value |
|---|---|
| Source IP | `10.10.57.178` |
| Source port | `46742` |
| Destination IP | `34.117.237.239` |
| Destination port | `443` |

The source port represents the temporary client-side port used for the connection.

Destination port 443 is commonly used for HTTPS and TLS-protected web traffic.

## 7. TLS Analysis

After the TCP connection was established, the capture showed TLS traffic.

This indicates that the communication was encrypted at the application layer.

Because the traffic was protected by TLS:

- Application data was not visible as readable plaintext
- The contents of the communication could not be directly inspected
- The investigation was limited to metadata and visible protocol information

## 8. Investigation Flow

The observed communication can be summarised as:

1. The client requests DNS resolution.
2. DNS resolves `contile.services.mozilla.com`.
3. The domain resolves to `34.117.237.239`.
4. The client sends TCP SYN.
5. The server sends TCP SYN/ACK.
6. The client sends TCP ACK.
7. TLS communication begins over port 443.

## 9. Conclusion

The capture showed a normal sequence of network communication:

- A client requested DNS resolution for a domain.
- The domain was resolved to an IP address.
- The client initiated a TCP connection to that IP address.
- The connection used destination port 443.
- TLS-protected communication followed the TCP handshake.

The analysis demonstrates the ability to correlate DNS activity with a subsequent network connection and identify the basic structure of TCP and TLS communication.

This investigation alone does not provide enough evidence to classify the traffic as malicious or benign. Additional context, payload analysis, endpoint information, and broader traffic analysis would be required for a security verdict.

## 10. Key Takeaways

- DNS can be used to connect a domain name with an IP address.
- The first SYN packet identifies the host initiating a TCP connection.
- The TCP three-way handshake establishes a reliable connection.
- Source and destination ports help identify the client side and target service.
- Port 443 is commonly associated with HTTPS and TLS.
- TLS encrypts application data and limits direct content inspection.
- Network observations should be documented separately from security conclusions.
