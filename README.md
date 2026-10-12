# Network Traffic Investigation

## Overview

This project documents a basic network traffic investigation performed with Wireshark.

The analysis focuses on:

- DNS resolution
- TCP connection establishment
- Source and destination ports
- TLS/HTTPS traffic
- The relationship between DNS activity and subsequent network communication

This is an introductory cybersecurity project created while learning the fundamentals of network traffic analysis.

## Learning Objectives

The main objectives of this investigation were to:

- Identify the client and server involved in a connection
- Analyse DNS queries and responses
- Understand the TCP three-way handshake
- Identify the destination service by examining traffic and ports
- Distinguish encrypted TLS traffic from plaintext application traffic
- Document network observations in a structured report

## Source

The packet capture used in this investigation was provided as part of the TryHackMe room:

**Wireshark: The Basics**

Room link: https://tryhackme.com/room/wiresharkthebasics

Capture file: `exercise.pcapng`

The original packet capture is not included in this repository. It is available through the original TryHackMe exercise.

## Tools

- Wireshark
- TryHackMe
- PCAPNG packet capture

## Methodology

The investigation was performed by opening `exercise.pcapng` in Wireshark and examining:

- DNS requests and responses
- Resolved IP addresses
- TCP connection establishment
- Source and destination ports
- TLS traffic
- The relationship between the resolved domain and the subsequent connection

Example Wireshark display filters used during the analysis:

- `dns`
- `tcp`
- `tcp.port == 443`
- `tls`
- `ip.addr == 34.117.237.239`

## Key Findings

The analysed traffic showed:

- A DNS request for `contile.services.mozilla.com`
- A DNS response resolving the domain to `34.117.237.239`
- A client host with the IP address `10.10.57.178`
- A DNS server with the IP address `10.0.0.2`
- A TCP connection from source port `46742` to destination port `443`
- A completed TCP three-way handshake
- TLS traffic following the TCP connection

The TCP handshake was observed in the following order:

- SYN — packet 16873
- SYN/ACK — packet 16874
- ACK — packet 16875

## Investigation Summary

The observed communication followed this sequence:

1. DNS query for `contile.services.mozilla.com`
2. DNS response with IP address `34.117.237.239`
3. TCP connection initiated by `10.10.57.178`
4. TCP connection to destination port `443`
5. TLS-protected communication

The first SYN packet was sent by `10.10.57.178`, identifying that host as the client initiating the connection.

The use of TLS indicates that the application traffic was encrypted. The contents of the protected application communication were therefore not directly visible as plaintext in the packet capture.

## Limitations

This project focuses on basic packet analysis.

The investigation does not attempt to:

- Attribute the traffic to a malicious actor
- Determine whether the communication was malicious
- Decrypt TLS-protected application data
- Perform malware analysis
- Conduct a full incident response investigation

Port 443 is commonly associated with HTTPS, but protocol identification should be based on packet analysis and protocol information, not only on the port number.

## What I Learned

Through this investigation, I practised:

- Navigating packet captures in Wireshark
- Filtering traffic by protocol
- Following DNS resolution
- Recognising the TCP three-way handshake
- Identifying client and server roles
- Recognising TLS-protected communication
- Connecting DNS activity with later network connections
- Writing a concise technical investigation report

## Related Report

Detailed observations and conclusions are available in [`investigation.md`](investigation.md).
