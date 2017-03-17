# XSSJacking
This is an attack that can trigger Self-XSS if the page in question is also vulnerable to Clickjacking.

Self-XSS is a type of XSS that typically can only be triggered by a user typing in an XSS payload which triggers on themselves. This can be DOM based, or set in a field only settable and viewable by the one user.

Clickjacking, is an attack that frames a website of a logged in user, typically sets the opacity of the frame to 0, and forces a victim to interact with their account, on a different website, unbeknownst to them.

They are both often excluded from bug bounty payouts. Here I will show a relatively practical way to exucute XSS payloads on sites vulnerable to both conditions


## How it works
This attack leverages [Pastejacking](https://github.com/dxa4481/Pastejacking) to force users to paste XSS payloads into text fields framed from other domains.

These frames can be redressed, made invisible, and overlayed ontop of other UI elements, making the user think they're interacting with another website.

## Demo
In this first site, there is a big input field that is vulnerable to Self-XSS [https://security.love/XSSJacking](https://security.love/XSSJacking)

To restrict people running Javascript on my domain, I have simplified the XSS to only pop an alert if `<script>alert(1)</script>` is entered into the field. 

![Self-XSS](https://i.imgur.com/Xu2N1Kx.png)

In this second site, [https://security.love/XSSJacking/index2.html](https://security.love/XSSJacking/index2.html) I have set up a scenario enticing people to copy paste things. Specifically, I've told a user to enter their email, and then repeat the email address on the next line. If the victim is anything like me, they will likely type their email once, copy it, and then paste it into the second field. This is where the Pastejacking comes in. After the copy, the contents of their clipboard get overwritten with `<script>alert(1)</script>`

The second email field is actually a cropped Iframe of the vulnerable site. When the victim goes to paste their email into the field, they'll actually paste the script tag, and trigger the XSS on the victim's domain

![XSSJacking](https://i.imgur.com/kXwa2jM.png)

## Conclusion
Though Clickjacking and Self-XSS are typically excluded from bug bounties, when both vulnerabilities are present, it isn't too difficult to craft a payload that forces the XSS to trigger on the victim.
