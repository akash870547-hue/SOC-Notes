# 02 · Phishing Analysis — Field Notes

**Best paired with:** [Phishing Analysis Fundamentals](https://tryhackme.com/room/phishingemails1tryoe), [Phishing Emails in Action](https://tryhackme.com/room/phishingemails2rytmuv), and [Phishing Analysis Tools](https://tryhackme.com/room/phishingemails3tryoe).

These notes cover transferable defensive analysis—not answers or a step-by-step solution to any active room.

## First rule: preserve, don't interact

Suspicious email ko forward, click ya attachment open karke investigate mat karo on a normal workstation. Follow the organization's approved reporting process and use isolated analysis tools. Never submit confidential email/attachments to public scanners unless policy explicitly permits it.

## Evidence checklist

### Message and identity
- Display name vs actual sender address
- From, Reply-To and Return-Path relationships
- Received chain and timestamp/timezone
- SPF, DKIM and DMARC results **in context**; one pass/fail is not a complete verdict
- Message-ID, sending infrastructure and mail gateway verdict where available

### Link and attachment indicators
- Visible link text vs actual destination
- Domain spelling, subdomain structure, unusual redirect chains and URL shorteners
- Attachment type, file name, hash and reputation context
- Macro/script/executable risk based on approved tooling
- Whether the same artefact appeared in other mailboxes or security alerts

### Business context
- Was the message expected?
- Is the sender normally known to the recipient?
- Does the request create urgency, payment changes, credential collection or secrecy?
- Was a similar message sent to multiple staff?

## A disciplined workflow

1. Record the reported message and preserve its metadata using approved tooling.
2. Extract sender, reply-to, URLs, attachment names and hashes without opening risky content.
3. Compare authentication results and mail gateway observations; don't confuse a valid sender domain with a harmless message.
4. Enrich indicators using approved tools and capture lookup time/source.
5. Correlate with proxy/DNS, endpoint process, identity and mail-delivery logs where available.
6. Determine whether the user clicked, entered credentials, executed an attachment or triggered a follow-on event—only from available evidence.
7. Recommend the approved next step: mailbox search/purge, indicator monitoring/blocking, identity reset or incident escalation, according to evidence and authority.
8. Document scope, limitations, actions and owner.

## What not to overclaim

- A look-alike domain is suspicious context, not proof of delivery or execution.
- SPF/DKIM/DMARC passing does not prove that a message's content is safe.
- A reputation tool returning no detections does not guarantee safety.
- An email being blocked does not establish that nobody received or interacted with a copy.

## Generic analyst note

~~~text
Message reference:
Recipient / scope:
Time + timezone:
Sender / Reply-To:
Authentication results:
URLs / domains:
Attachment metadata / hashes:
Mail-gateway evidence:
Endpoint / identity / network correlation:
User interaction evidence:
Assessment + confidence:
Actions / approvals:
Remaining gaps:
~~~

## Hinglish takeaway

Phishing mein sender, content, infrastructure aur user-impact ko ek story ki tarah correlate karo. Sirf ek red flag se conclusion mat likho—and missing evidence ko clearly mention karo.
