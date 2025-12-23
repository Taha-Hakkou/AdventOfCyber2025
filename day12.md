## day 12

* spam vs phishing
- Spam is just digital noise: annoying, but mostly harmless. Phishing, however, is a precision strike.
- Spam focuses on quantity over precision.

* common phishing techniques:
- impersonation (person/department/service)
- Social Engineering
- Typosquatting and Punycode
1. Typosquatting is when an attacker registers a common misspelling of an organisation's domain (typos, missing character, letters reversed...).

2. punycode is a special encoding system that converts Unicode characters (used in writing systems like Chinese, Cyrillic, and Arabic) into ASCII (accepted format by DNS). example:
**tryhackme.com** domain written with **тrу** instead (Cyrillic т, Cyrillic г, Cyrillic у) results in: **xn--hackme-oof3jk.com** where:
xn--: ACE prefix, -oof3jk: encoded non-ascii chars
An easy way to identify punycodes is by looking at the field Return-Path in the email headers.

3. Spoofing:
The message looks like it came from a trusted sender (the display name and “From:” you see in the preview), but the underlying headers tell a different story. Modern email clients can easily reject spoofing attempts.
Checking some essential fields in the email headers: Authentication-Results, Return-Path..
On Authentication-Results, SPF, DKIM, and DMARC are security checks that help confirm if an email really comes from who it says it does:
**SPF**: Says which servers are allowed to send emails for a domain (like a list of approved senders).
**DKIM**: Adds a digital signature to prove the message wasn’t changed and really came from that domain.
**DMARC**: Uses SPF and DKIM to decide what to do if something looks fake (for example, send it to spam or block it).
If both SPF and DMARC fail, it’s a strong sign the email is spoofed.
On the Return-Path we can see the real mail address.

4. Malicious Attachments
Malicious attachments can have multiple goals, either installing malware, stealing passwords, or giving attackers access to the device or network.
example: HTA/HTML files are commonly used for phishing because they run without browser sandboxing, meaning scripts have full access to the endpoint they execute on!


* trending phishing:
In short, most phishing attacks aren’t about dropping malware directly; they’re focusing on stealing access using:

- legitimate apps (Dropbox, Google Drive/Docs, and OneDrive...)

- fake logon pages

- malicious files

- fake invoices

- Side Channel Communications
attacker moves the conversation off email to another channel, such as SMS, WhatsApp/Telegram, a phone or video call, a texted link, or a shared document platform, to continue the social engineering in a platform without the company's control.