# Sample Message Analysis Report

**DecodeLabs Project 3 - Phishing Awareness Analysis**

Scope: six original synthetic messages, inspected as text only. `.example`
addresses are training placeholders. All URLs are deliberately defanged.
There are no real victims, active phishing sites, or downloaded attachments.

## Results overview

| ID | Scenario | Assessment | Confidence |
| --- | --- | --- | --- |
| S01 | Account suspension and password request | SIMULATED PHISHING | High |
| S02 | Delivery fee and deceptive link destination | SIMULATED PHISHING | High |
| S03 | Executive impersonation and gift cards | SIMULATED PHISHING | High |
| S04 | Unexpected invoice with macro instructions | SIMULATED PHISHING | High for social engineering; payload unknown |
| S05 | Unrecognized document-sharing notification | UNCONFIRMED | Insufficient context |
| S06 | Expected workshop reminder | LOW CONCERN / UNVERIFIED | Moderate; identity not authenticated |

These are qualitative training judgments. They are not detection accuracy
measurements or confirmed real-world security findings.

## S01 - Account suspension lure

Scenario context: the fictional bank's known portal is `portal.harborbank.example`.

```text
From: Harbor Bank Security <alerts@harborbank-security.example>
Reply-To: verification@account-review.example
Subject: Immediate action required: account suspended in 30 minutes

Dear customer,
Verify your account now at:
hxxps://harborbank.example.account-check.example/verify
Reply with your password and one-time code to prevent permanent closure.
```

| Evidence | Red flag and reasoning |
| --- | --- |
| `harborbank-security.example` | Sender domain differs from the known bank service; the display name alone is not identity proof. |
| `verification@account-review.example` | Replies containing secrets would go to another unrelated training domain. |
| `harborbank.example.account-check.example` | Actual hostname ends in `account-check.example`; `harborbank.example` is only a misleading prefix. It is not the known portal. |
| `30 minutes`, `permanent closure` | Threats and a short deadline pressure the recipient to skip verification. |
| `password and one-time code` | Directly requests authentication secrets. |

**Assessment:** SIMULATED PHISHING, high confidence. The decisive evidence is
the secret request combined with identity and destination inconsistencies;
the generic greeting is only supporting context.

**Why unsafe / potential impact:** disclosing both a password and a current
code could enable account takeover. A bank-looking word inside a hostname
does not establish control by that bank.

**Safe response / next check:** do not reply or use the supplied URL. Check the
account through the previously known portal and report the original message.
For a real incident, the security team would examine full headers and delivery
context; no such authentication evidence is supplied here.

## S02 - Delivery fee with misleading link text

Scenario context: the recipient has no expected parcel from this fictional courier.

```text
From: Parcel Desk <delivery@parcel-alert.example>
Subject: Final notice: pay the small delivery fee today

Your parcel is on hold. Pay 1.99 today to avoid return to sender.
Visible link text: https://parcel.example/track
Actual target: hxxps://parcel.example@delivery-check.example/pay
Enter your full card number and security code to release your parcel.
```

| Evidence | Red flag and reasoning |
| --- | --- |
| No expected delivery | The requested transaction has no established context. |
| Visible text vs actual target | The shown courier URL differs from the destination. |
| `parcel.example@delivery-check.example` | Text before `@` is userinfo; the hostname is `delivery-check.example`, not `parcel.example`. |
| `Final notice`, `today`, `return to sender` | Deadline and loss pressure encourage a hurried payment. |
| Full card number and security code | Sensitive payment data is requested through an unverified destination. |

**Assessment:** SIMULATED PHISHING, high confidence. The visible/actual link
mismatch and unrelated destination provide stronger evidence than the fee alone.

**Why unsafe / potential impact:** the scenario attempts to divert the user to
a payment-data collection page. A small advertised fee does not limit the
possible harm from exposing card details. HTTPS would not prove the destination
belongs to the courier.

**Safe response / next check:** do not pay through the message. Check any real
order using the merchant account or the courier's independently known tracking
service. Do not open the target simply to see where it leads.

## S03 - Executive impersonation without a link

Scenario context: gift-card purchases are outside the fictional team's normal
approval process; the displayed executive is reachable through a known directory.

```text
From: Maya, Managing Director <maya.office@external-mail.example>
Subject: Confidential favor needed now

I am in a meeting and cannot take calls. Buy five gift cards immediately
and reply with photos of the redemption codes. Keep this between us;
do not involve finance. I will reimburse you later.
```

| Evidence | Red flag and reasoning |
| --- | --- |
| Executive display name with external address | Claimed authority is not supported by an authenticated corporate identity. |
| `cannot take calls` | Discourages independent verification. |
| `gift cards`, `redemption codes` | Requests transferable value through an unusual business payment method. |
| `Keep this between us`, `do not involve finance` | Explicitly attempts to bypass oversight and normal approval. |
| `immediately`, `reimburse you later` | Combines time pressure with an assurance intended to reduce hesitation. |

**Assessment:** SIMULATED PHISHING / payment social engineering, high confidence.
There is no URL or attachment; their absence does not make a message safe.

**Why unsafe / potential impact:** sending redemption codes could allow the
recipient of those codes to spend the value, producing financial loss.

**Safe response / next check:** verify through the existing internal directory
and use the normal finance approval process. Do not reply to the suspicious
sender to verify their own identity. Preserve and report the message.

## S04 - Invoice attachment and macro pressure

Scenario context: the recipient did not request an invoice or purchase from this vendor.

```text
From: Vendor Billing <billing@vendor-payments.example>
Subject: Overdue invoice - immediate payment required
Attachment name: Invoice_2026.xlsm

Your unpaid invoice is attached. Open it and select Enable Content
to view the amount. If a security warning appears, ignore it; the file
is approved. Payment is due today.
```

| Evidence | Red flag and reasoning |
| --- | --- |
| Unexpected invoice | No known transaction supports the request. |
| `.xlsm` | A macro-enabled workbook can contain active code; the extension alone is not proof of malware. |
| `Enable Content` | Attempts to persuade the recipient to allow active content in an unverified file. |
| `ignore` a security warning | Directly asks the recipient to bypass a protective decision point. |
| `approved`, `due today` | Unsupported reassurance and time pressure reinforce the request. |

**Assessment:** SIMULATED PHISHING, high confidence for the social-engineering
attempt. **Attachment maliciousness is UNCONFIRMED:** only a filename and
message text exist in this exercise; there is no file to inspect.

**Why unsafe / potential impact:** following the instructions could allow
malicious macros to run if such a payload existed and the application permitted
it. This report does not claim execution, infection, or data theft occurred.

**Safe response / next check:** do not open or enable the attachment. Confirm
the invoice using established vendor records. In a real incident, an authorized
security team could inspect the actual file in an isolated analysis environment.

## S05 - Ambiguous sharing notification

Scenario context: a file-sharing service might legitimately be used by the team,
but the recipient cannot recall the document or sender.

```text
From: Team Share <notifications@teamshare.example>
Subject: A document has been shared with you

Jordan shared "Quarterly planning notes" with you.
Open the document: hxxps://teamshare.example/document/4821
```

| Evidence | Observation and uncertainty |
| --- | --- |
| Unrecognized document | Unexpected context warrants verification, but is not proof of phishing. |
| Sender and URL both use `teamshare.example` | Consistency removes one visible mismatch; it does not authenticate the service or sender. |
| No secret request or deadline | Some common warning signs are absent, which cannot exclude a carefully written lure. |

**Assessment:** UNCONFIRMED. There is insufficient evidence to call the
notification malicious or legitimate.

**Potential impact if malicious:** an imitation sharing flow could solicit
credentials or lead to an unwanted download. Neither behavior is observed here.

**Next check needed:** ask Jordan through an established contact channel and
look for the document by independently opening the team's known sharing portal.
Review full email headers and tenant/service context if uncertainty remains.
Do not classify the link as malicious merely because its domain is unfamiliar.

## S06 - Expected reminder as a comparison

Scenario context: the recipient registered for this workshop, recognizes the
organizer, and the meeting is already visible in their existing calendar.

```text
From: Training Team <training@academy.example>
Subject: Reminder: your scheduled security workshop

Hello Amirbek,
Your workshop starts tomorrow at 10:00. The agenda is already available
in your existing learning portal and calendar. No payment or reply is needed.
Thank you,
Training Team
```

| Evidence | Observation and reasoning |
| --- | --- |
| Expected registration and calendar event | Independent prior context supports the message. |
| Existing portal rather than an embedded login URL | The recipient can confirm the details without following a new destination. |
| No secrets, payment, attachment, or policy bypass requested | No obvious harmful action is requested in the supplied text. |

**Assessment:** LOW CONCERN / UNVERIFIED. The scenario is consistent with a
routine reminder; the sender identity has not been authenticated from headers.
A personalized greeting alone is not proof of legitimacy.

**Safe response / next check:** consult the existing calendar or learning portal
if confirmation is needed. Treat any later request for payment or credentials
as a new message requiring its own assessment.

## Conclusions

The four simulated phishing cases use different mechanisms: secret harvesting,
payment-data collection, gift-card fraud, and pressure to enable active content.
The analysis ties each judgment to a requested action and quoted evidence rather
than treating a keyword or a URL's appearance as sufficient proof.

S05 needs independent context before a verdict; S06 illustrates lower concern
without promising safety. For real messages, preserve evidence and escalate
through the organization's approved reporting process.
