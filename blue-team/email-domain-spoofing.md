# Email domain spoofing

## 1) Sender Policy Framework (SPF)

SPF, or Sender Policy Framework, is an **email authentication protocol** designed to prevent **email spoofing**. It allows the owner of a domain to specify which **mail servers are authorized** to send email on behalf of that domain. When an email is received, the receiving server can check the **SPF record of the sender's domain** to verify if the email is coming from an authorized server.

It is a TXT type record used to register all the IP addresses authorized to send emails for a particular domain.

Adding a TXT type record in the DNS record according to the SPF format will increase the credibility of the domain and prevent spam emails from spoofing the domain's sender to send spam emails.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

The content in the red box is a typical SPF record, specifying the allowed domain names. The meaning of each field is as follows:

* `v=spf1` is the identifier for the SPF record, where `1` indicates the version of SPF. If using "Sender ID," this field should be `v=spf2`.
* `include:spf1.staff.mail.aliyun.com` specifies which third-party organizations are allowed to send emails on behalf of this domain.
* `-all` indicates that addresses not listed in the SPF record are **not authorized.**

## 2) DomainKeys Identified Mail (DKIM)

DKIM (DomainKeys Identified Mail) is an email authentication method designed to detect forged sender addresses in emails. It allows the recipient to verify that an email was indeed sent and authorized by the owner of that domain. This is achieved through cryptographic authentication.

To determine if DKIM is configured, you can check the email headers for the presence of a `DKIM-Signature`

