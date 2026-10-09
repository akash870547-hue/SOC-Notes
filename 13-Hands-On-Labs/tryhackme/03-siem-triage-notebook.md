# 03 · SIEM Triage Notebook — Field Notes

**Best paired with:** [Introduction to SIEM](https://tryhackme.com/room/introtosiem), [Splunk: The Basics](https://tryhackme.com/room/splunk101), [Elastic Stack: The Basics](https://tryhackme.com/room/investigatingwithelk101), [Log Analysis with SIEM](https://tryhackme.com/room/loganalysiswithsiem), [Alert Triage With Splunk](https://tryhackme.com/room/alerttriagewithsplunk) and [Alert Triage With Elastic](https://tryhackme.com/room/alerttriagewithelastic).

Original generic queries below use sample field names. They are **not** copied room answers and may need adapting to the lab's schema.

## Before querying

Know your data source, index/data view, time window, timezone, ingest delay and field names. Queries that return zero rows can mean “no match,” “wrong fields/index,” or “no data”; verify which before drawing conclusions.

## Search pattern: authentication failures

~~~spl
index=lab earliest=-24h EventCode=4625
| stats count as failures min(_time) as first_seen max(_time) as last_seen by TargetUserName, IpAddress, Computer
| sort - failures
~~~

Use it as a starting point to see *where* failures cluster. Then inspect raw records and look for successful logons, logon type, status fields, expected VPN/NAT behaviour and account baseline. Do not treat an arbitrary threshold as universal.

## Search pattern: process context

~~~spl
index=lab earliest=-4h EventCode=4688
| table _time Computer SubjectUserName NewProcessName ProcessCommandLine ParentProcessName
| sort 0 _time
~~~

Process-command-line and parent-process fields may be absent unless configured. Check raw events or EDR telemetry before concluding that a field is empty in the underlying activity.

## Search pattern: DNS volume

~~~spl
index=lab sourcetype=dns earliest=-24h
| stats count dc(query) as unique_queries by src_ip
| sort - count
~~~

A high count or many unique names can justify a pivot; it does not prove tunnelling or C2. Compare with a baseline, endpoint-process data, destination context and plausible software/CDN behaviour.

## Search pattern: web response codes

~~~spl
index=lab sourcetype=web earliest=-4h
| stats count by src_ip uri_path status
| sort - count
~~~

Use this to find clusters. Review requests, user-agent, authentication state, WAF/proxy action, application logs and follow-up requests before assigning intent.

## Query-quality checklist

- Narrow time window and relevant data source first.
- Keep the original alert query for reproducibility.
- Inspect raw events after aggregation.
- Distinguish event count from distinct users/hosts.
- Validate field aliases and timezone.
- Record filters, assumptions and missing data.
- Test threshold/rule changes against positive, benign and edge-case samples.

## Notes I keep for every query

~~~text
Hypothesis:
Index / data view / source:
Time window + timezone:
Fields used:
Filters and reason:
Result summary:
Raw events validated?:
Benign explanation checked:
Known gaps / performance concerns:
Next pivot:
~~~

## Hinglish takeaway

SIEM ka kaam sirf “search result mil gaya” nahi hai. Analyst ko explain karna aana chahiye ki query ne kya filter kiya, result kya prove karta hai—and kya prove nahi karta.
