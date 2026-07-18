# Phishing Email Analysis Lab

## Overview
Analyzed real, documented phishing email examples from UC Berkeley's public Phishing Examples Archive to practice spotting red flags and understand the different tactics attackers use to trick people into giving up credentials or personal information.

## Objective
To identify red flags in real phishing emails on my own first, then compare my analysis against a trusted source's official explanation — building the kind of judgment a SOC analyst needs when triaging a reported phishing email.

## Approach
This lab is manual analysis, not tool-based — no scanning or SIEM tools were used here. Instead of using live phishing emails (risky), I used a public archive of phishing attempts UC Berkeley has already received, documented, and explained. For each example, I tried to spot the red flags myself first, then checked the archive's own explanation to see what I got right and what I missed.

## Source
UC Berkeley Information Security Office — Phishing Examples Archive (security.berkeley.edu)

## Examples Analyzed

### 1. Fake Assessment Report Email
**Sender:** Cynthia Burns `<cburns13@gapps.bcsds.org>`
**Subject:** "Please Review Your 2025 Assessment Report"

**What I noticed:**
- The sender's domain (`gapps.bcsds.org`) isn't a real, recognizable company domain
- "Hello Team" instead of addressing anyone by name
- Signed off vaguely as "HR Department," no actual person's name
- The "Open My Assessment Report" link doesn't show where it actually goes
- Mentions "role or rank adjustments" — trying to get you curious enough to click

**What the archive confirmed:**
- The sender wasn't an actual `@berkeley.edu` address — usually Gmail, Outlook, or similar instead
- The link led to a fake login page that wasn't the real CalNet login
- The same scam was sent under a few different subject lines, so it's not just one fixed template
- If it looks off, the advice is to just verify with the person separately, or report it using "Report phishing" in the email client

**Verdict:** Phishing — trying to steal login credentials through a fake report link.

### 2. Fake Fax Notification
A "Fax Notification" style email with a sender fax number and a "View Fax" button.

**What I noticed:**
- No real explanation of who's sending this fax or why — just a vague sender label ("SMCH")
- Recipient field just shows a generic address, not sent to me specifically
- The "View Fax" button hides where it actually leads
- Has a fake "this may contain confidential information" disclaimer at the bottom, probably added just to look more official
- Doesn't feel urgent, more just mildly curious — a different approach than the usual "act now" phishing tone

**Verdict:** Phishing — clicking through likely leads to a fake login page.

### 3. Fake Electronic Payment (ACH) Message
This one pretends to be from a UC Berkeley IT technician, warning that email access will be lost unless action is taken.

**What the archive explained:**
- Pretends to be an internal IT person instead of some random outsider
- Uses fear — threatens losing email access
- Comes from a Gmail address instead of an actual university domain
- Uses made-up-looking reference numbers in the subject line (like "#658-745") to seem more legitimate
- Their advice, which is a good rule in general: never enter your real login credentials on any page that isn't the official university login page

**Verdict:** Phishing — using fear about losing access to get people to hand over credentials.

### 4. Fake Email Account Suspension Email
**Subject:** "ADVANCE WARNING"

Claims the account is on a list for deactivation and needs to be "verified" immediately, signed as "UNIVERSITY OF CALIFORNIA BERKELEY SERVICE DESK TEAM."

**What I noticed:**
- Classic fear tactic — threatens losing your account if you don't act fast
- Vague reason given ("filed under the list of accounts set for deactivation") with no real specifics
- No actual person's name, just "Service Desk Team"
- Adds a line saying it's a "one-time submission" — trying to stop you from pausing to think it over

**The actual fake login page — this was the most convincing part:**
I actually got to see where the link leads: a page at `docs.google.com/forms/d/e/...`, but the page itself is styled to look exactly like a Microsoft Office 365 login screen ("Welcome to Office"). This was honestly the clearest example for me — the page looked completely legitimate, but the web address gave it away immediately as fake, since a real Office login would never be hosted on a Google Forms link.

**What the archive confirmed:**
- Berkeley Help Desks never contact people by text to a personal phone
- No real technician will ever ask for your password or login code over email/text
- Never enter your real login on anything that isn't the official university login page — including things styled like Google Forms or Office logins, exactly like this one
- Suspicious texts should be screenshotted and sent to `phishing@berkeley.edu`

**Verdict:** Phishing — confirmed by the mismatched web address behind a very convincing fake login page.

### 5. Fake Internship Offer (Cal-1 Card / ID Theft)
**Sender:** "Prof. Stephen P. Hinshaw" `<hinshaw.berkeley.edu@gmail.com>`
**Subject:** "Inquiry Received!"

Pretends to be a real UC Berkeley professor offering a remote research assistant position — $350/week, under 7 hours a week, no experience needed — then asks for a scan of the applicant's student ID card and resume to "proceed."

**What I noticed:**
- The sender address is genuinely sneaky — `hinshaw.berkeley.edu@gmail.com` has "berkeley.edu" stuffed into the username part, but the real domain is just gmail.com. I almost missed this one myself; it looks legit at a quick glance.
- The offer is too good for what's asked — high pay, barely any hours, no experience required, no real interview process — this is meant to get someone excited enough to not think it through
- Asking for a student ID scan is a big flag — that card has an ID number, barcode, and gives access to buildings, meals, and stored funds
- Uses a real professor's actual name instead of a made-up one, which makes it feel more convincing than the other examples

**What the archive confirmed:**
- No real UC Berkeley job offer comes through a non-university email, personal phone number, Instagram, or WhatsApp
- Sensitive documents/ID scans should never be requested over email
- A real campus job will never ask you to send money at any point

**Verdict:** Phishing — trying to steal ID card information, disguised as a legit internship offer, using a real person's name and a sneaky look-alike email address.

## Evidence
![Fake Assessment Report email](./screenshots/phishing-assessment-report.png)

![Fake Fax Notification email](./screenshots/phishing-fax-notification.png)

![Fake Account Suspension email](./screenshots/phishing-account-suspension-email.png)

![Fake Office 365 login page](./screenshots/phishing-fake-office-login.png)

![Fake internship offer email](./screenshots/phishing-internship-scam.png)

## Key Findings
- All five examples used a hidden link, a fake login page, or a direct request for sensitive information as the actual attack method.
- Each one used a different angle to get someone to act: curiosity (assessment report, fax), fear (payment/service loss, account suspension), and an attractive offer (internship) — not just one style of manipulation.
- Every example had a sender domain that didn't match who it claimed to be from — sometimes obvious (a random domain), sometimes disguised to look real at a glance (berkeley.edu stuffed into a Gmail address).
- The account suspension example was the only one where I could actually see the fake login page itself, which made the "check the URL, not how official it looks" lesson much more concrete.

## Insights / Analysis
- Checking the sender's actual domain is the fastest and most reliable check — but it's not always obvious at a glance, since some addresses are deliberately built to look legitimate.
- The same phishing template (fake assessment report) was reused across a few different subject lines in the real campaign, which tells me detection shouldn't rely on memorizing exact wording, just recognizing the pattern.
- Seeing the real fake Office 365 page made it clear that a page can look completely convincing while sitting on a totally unrelated, mismatched web address.
- Not every phishing email relies on urgency — the internship one worked through appeal and opportunity instead, which means I need to stay alert even when an email doesn't feel threatening.
- If this were a real reported alert, next steps would be checking the sender domain, checking where any link actually leads (using something like VirusTotal), and checking if others in the organization got the same email.

## What I Learned
The biggest takeaway was that checking the sender's actual email domain is the single fastest, most reliable check — but I also learned it's not foolproof at a glance, since some addresses are deliberately built to look legitimate. Seeing the real fake Office 365 login page was probably the most useful part of this lab, because it made the "check the URL, not how official it looks" idea click in a way that just reading about it never would have. I also learned that phishing doesn't always rely on fear — sometimes it's curiosity, sometimes it's an appealing offer — so I need to stay alert regardless of the tone of the email.

## Skills Demonstrated
`Phishing Analysis` `Social Engineering Pattern Recognition` `Email/Domain Verification` `Cross-Referencing Findings Against a Trusted Source`
