# Phishing Awareness Analysis

**DecodeLabs Cyber Security Internship - Project 3 | Batch 2026**

An evidence-based analysis of six synthetic emails/messages: four simulated
phishing attempts, one ambiguous notification, and one low-concern comparison.
Each case identifies suspicious links or wording, lists red flags, explains
the potential impact, and recommends a safe response.

## Read the project

Open [ANALYSIS.md](ANALYSIS.md) for the complete sample messages and analysis.
No installation, accounts, credentials, or executable attachments are needed.

All organizations, addresses, and messages in the cases are fictional. URLs
are defanged (`hxxps` and `[.]`) and use reserved `.example` names. No links
were visited, messages sent, attachments executed, or external systems tested.
The findings describe training scenarios, not actual incidents or accusations.

## Requirement coverage

| Project 3 requirement | Evidence |
| --- | --- |
| Identify suspicious links or keywords | Cases S01-S04 quote the relevant wording and URL components |
| List red flags in phishing messages | Each case has an evidence/reason table |
| Explain why the message is unsafe | Each case states the attempted deception and potential impact |
| Demonstrate security thinking | S05-S06 distinguish uncertainty from proof and avoid automatic safe verdicts |

## Analysis method

1. Read the message as text; do not activate its links or attachments.
2. Separate the display name from the actual From and Reply-To addresses.
3. Inspect URL structure, destination hostname, and displayed link text.
4. Identify the requested action: credentials, payment, secrets, or execution.
5. Correlate indicators with the stated scenario; one keyword alone is not proof.
6. Record evidence, confidence, impact, and the next verification step.

Labels used: **SIMULATED PHISHING**, **UNCONFIRMED**, and
**LOW CONCERN / UNVERIFIED**. Confidence is an analyst judgment about these
constructed scenarios, not a measured detection score.

## Employee red-flag checklist

- [ ] Does the sender address fit the claimed organization and expected contact?
- [ ] Does Reply-To redirect a sensitive conversation somewhere unexpected?
- [ ] Does the actual link destination match the visible text and known service?
- [ ] Is a brand name hidden in a subdomain, path, or URL userinfo section?
- [ ] Does the message ask for passwords, one-time codes, or banking details?
- [ ] Does it combine urgency, threats, rewards, secrecy, or authority pressure?
- [ ] Does it request an unexpected payment or a change in payment details?
- [ ] Does an attachment require macros or bypassing a security warning?
- [ ] Can the request be confirmed using a previously known channel?

Treat checked items as reasons to investigate, not an automatic verdict.
HTTPS, familiar logos, correct grammar, and a matching sender address do not
establish legitimacy. An unfamiliar domain or urgent wording alone does not
establish malicious intent either.

## Safe response

Pause interaction. Verify through a bookmarked official portal or a previously
known contact, rather than the message's own links or phone numbers. Use your
organization's report-phishing process and preserve the original message for
the security team. If credentials were shared, notify the team promptly and
secure the account through the known official service.

## Limitations

This is a manual awareness exercise, not a phishing detector or a malware
analysis. No full email headers, SPF/DKIM/DMARC results, URL reputation,
redirect chains, domain ownership records, or attachment contents were
available. A delivered message requires additional context and investigation;
even authenticated email can come from a compromised account.

## Sources

The examples and case judgments are original synthetic work. The general
awareness and independent-verification approach is informed by:

- [FTC: How To Recognize and Avoid Phishing Scams](https://consumer.ftc.gov/articles/how-recognize-avoid-phishing-scams)
- [Login.gov: Verify messages](https://www.login.gov/help/fraud-concerns/verify-messages/)

## Author and related projects

[Amirbek Raxmatullayev](https://github.com/americooo)

- [Project 1 - Password Strength Checker](https://github.com/americooo/DecodeLabs-Password-Strength-Checker)
- [Project 2 - Basic Encryption & Decryption](https://github.com/americooo/DecodeLabs-Basic-Encryption-Decryption)
