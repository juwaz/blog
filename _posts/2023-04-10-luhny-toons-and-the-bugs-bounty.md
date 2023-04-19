---
toc: true
layout: post
description: revisting my bug bounty experience
categories:
  [information security, infosec, bug bounty, luhn algorithm, opinion piece]
title:
  bug bounties are filled with challenges even after a vulnerability is found
---

# bring me the head of your authentication portal

![bring me the head of alfredo garcia blu-ray insert]({{ site.baseurl }}/images/cabeza.jpg
"bring me the head of alfredo garcia blu-ray insert")

## bug bounty primer

For those unfamiliar with the term, bug bounties are programs that security
conscious organizations offer for "White Hat" hackers to review their technology
systems for vulnerabilities and engage in responsible disclosures in exchange
for compensation and recognition. Historically security researchers complain
that most companies don't have a well structured or maintained program and
seemingly work against the white hat hacker at every turn.

## luhn's primer

```python
def luhn(n):
		r = [int(ch) for ch in str(n)][::-1]
		return (sum(r[0::2]) + sum(sum(divmod(d*2,10)) for d in r[1::2])) % 10 == 0

```

[Hans Peter Luhn's algorithm](https://en.wikipedia.org/wiki/Luhn_algorithm) also
known as the MOD 10 is the go to checksum for validating identification numbers
found most notably in credit card processing [^1] but also appearing in a range
of identification numbers from telephony to social security.

## the find

During an online hackathon I discovered that a major billing processor had an
issue with one of their portals processing the luhn's credit card number
validations in a snippet of their **client side code** (readily accessible to
any end user of the web app). This interaction with the portal could be
**fuzzed** (substituting random input values until a new or desired output
returns). This portal lacked rate limitations, robot validations, or malicious
IP address filtering. This in combination with another hosted portal where
ordinary users could retrieve the first six and last four of their credit card
(for convenience purposes) meant that the billing company essentially provided
the hash and payload of customer credit cards (the nature of luhn's algorithm)
as well as an open and unlimited portal to test them.

## antics ensue

After documenting the issue and reaching out to the billing company requesting a
responsible disclosure/bug bounty path I met with hardships at every turn. First
I didn't hear anything for weeks and when I re-engaged them for a follow-up I
received a curt response that the issue was out of scope for their program which
targeted exploitation of a specific resource rather than compromising users by
combining disparate portals. I responded that this out of the box approach to
extracting sensitive information mirrors how a "black hat" would approach
attacking their system/users and that their scope should expand accordingly.
Once again I heard nothing initially but found an offer for company "Swag"
buried in my email weeks later. I declined as I had no interest in promoting the
company and intended to pursue other avenues to submit/appeal the bounty
(sometimes soliciting intervention from an established bug bounty company like
hackerone can lead to improved outcomes for the white hat). Soon after their
last contact offering "Swag" I noticed that both the client side Luhn's
vulnerability and the convenience portals updated to feature rate limits and
optical character recognition prompts to deter malicious prompts/inputs.

## that's all folks

To recap the billing company stalled throughout the process, defined scope in a
way that benefited them and offered pennies for a vulnerability that was
certainly worth dollars to a black hat or in the wild. From what I can tell this
behavior encompasses the **_rule_** not the **_exception_** when it comes to
most bug bounty programs. In short the incentives between the organization and
the hacker are not aligned and even in the best run programs the white hats are
under appreciated. Regardless I have no regrets and found the process of finding
and attempting to responsibly disclose the bug bounty a fun and rewarding
experience.

![bugs bunny and daffy duck 8 bit that's all folks](https://media.tenor.com/C8XjQ4wtqO0AAAAC/thats-all-folks-game-over.gif "bugs bunny and daffy duck 8 bit that's all folks")

## footnote

[^1]: source: https://www.creditcardvalidator.org/articles/luhn-algorithm
