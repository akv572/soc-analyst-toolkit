# Wireshark Display Filters

The filters I reach for when triaging suspicious traffic captures.

## Basics

```
ip.addr == 192.0.2.10          # all traffic to/from a host
ip.src == 192.0.2.10
tcp.port == 443
udp.port == 53
http or dns or tls
```

## Finding suspicious behaviour

```
# DNS queries that are not to your internal resolvers
dns and not ip.dst == 10.0.0.53

# Long or encoded DNS labels (possible tunnelling)
dns.qry.name len > 40

# HTTP POSTs (data leaving the network)
http.request.method == POST

# TLS handshakes to IPs without SNI, or rare JA3 hashes
tls.handshake.extensions_server_name == ""

# Non standard ports carrying HTTP
tcp.port != 80 and tcp.port != 443 and http

# Large outbound transfers
tcp.len > 10000 and ip.src == 192.0.2.10
```

## Credentials and cleartext

```
# Basic auth headers, FTP logins, Telnet
http.authbasic
ftp.request.command == "USER" or ftp.request.command == "PASS"
telnet
```

## Triage workflow

1. `Statistics > Protocol Hierarchy` for a feel of the capture.
2. `Statistics > Conversations`, sort by bytes, investigate the top talkers.
3. Filter to one conversation, `Follow > TCP Stream` to read it.
4. `File > Export Objects > HTTP` to pull transferred files for sandboxing.
