# Day 2 (Phishing)

* Phishing is a subset of social engineering in which the communication medium is mostly messages.

* two anti-phishing mnemonics written as S.T.O.P:
    - The first S.T.O.P. is from All Things Secured, which tells users to ask the following questions before acting on an email:
        + Suspicious?
        + Telling me to click something?
        + Offering me an amazing deal?
        + Pushing me to do something now?
    - The second S.T.O.P. reminds users to follow the following instructions:
        + Slow down. Scammers run on your adrenaline.
        + Type the address yourself. Don’t use the message’s link.
        + Open nothing unexpected. Verify first.
        + Prove the sender. Check the real From address/number, not just the display name.

* __[Social-Engineer Toolkit (SET)](https://github.com/trustedsec/social-engineer-toolkit)__ is an open-source tool primarily designed by David Kennedy for social engineering attacks. It offers a wide range of features. In particular, it lets you compose and send a phishing email.

## Practical:

```sh
setoolkit
set> 1  # Social-Engineering Attacks
# option 99 takes you back to the main menu
# if you commit any mistake while writing the phishing email, press "Ctrl + C" to return to the main menu.
set> 5  # Mass Mailer Attack
set:mailer> 1  # E-Mail Attack Single Email Address
set:phishing> ...
```
