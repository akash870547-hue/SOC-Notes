# 02 · Network Security for SOC Analysts

## Read a connection as a tuple

Start with **source IP:port → destination IP:port + protocol + time**. Add action, DNS name, bytes, duration, application, user and sensor context where available.

## Protocol quick reference

| Protocol | Common port | SOC questions |
|---|---:|---|
| DNS | UDP/TCP 53 | What name was queried? Was the response unusual? |
| HTTP | TCP 80 | What host, URI, method, status and user-agent were logged? |
| HTTPS | TCP 443 | What can metadata reveal, and what remains encrypted? |
| SSH | TCP 22 | Is remote access expected? Which account and source? |
| RDP | TCP 3389 | Is the source authorized? Which logon events follow? |
| SMB | TCP 445 | Is this lateral access or ordinary file-share use? |
| LDAP / LDAPS | TCP 389 / 636 | Is directory traffic consistent with expected activity? |
| NTP | UDP 123 | Are clock offsets affecting the timeline? |

Ports are clues, not proof: services can run on nonstandard ports.

## Network investigation pivots

1. Bound the time range and preserve the original query.
2. Identify directionality and sensor vantage point.
3. Pivot from IP to DNS, then domain to other clients and periods.
4. Compare volume, frequency, duration, response code and bytes with baseline.
5. Correlate connections with endpoint processes or identity activity where possible.
6. Record sensor gaps: NAT, proxies, shared IPs, encryption, missing logs and retention limits.

## Example: unusual DNS volume

Ask whether the domain is newly seen, labels unusually long or high entropy, behavior isolated or distributed, an endpoint process explains it, and benign causes such as telemetry agents or CDNs are plausible.

## Example filters (adapt fields to your data)

**DNS pivot**
~~~spl
index=network sourcetype=dns earliest=-24h
| stats count dc(query) as unique_queries by src_ip
| sort - count
~~~

**Firewall review**
~~~spl
index=network sourcetype=firewall earliest=-4h
| stats count sum(bytes_out) as bytes_out by src_ip dest_ip dest_port action
| sort - count
~~~

These are illustrative. Confirm the index, sourcetype and field mapping before use.

## Why IP reputation is not enough

Feeds can have stale entries and shared hosting/CDNs serve unrelated customers. Combine reputation with behavior, timing, endpoint context and business relevance. Preserve source and lookup time for enrichment results.
