# Methods to prevent Email domain spoofing

## 0x01 Sender Policy Framework (SPF)

SPF, or Sender Policy Framework, is an **email authentication protocol** designed to prevent **email spoofing**. It allows the owner of a domain to specify which **mail servers are authorized** to send email on behalf of that domain. When an email is received, the receiving server can check the **SPF record of the sender's domain** to verify if the email is coming from an authorized server.

It is a TXT type record used to register all the IP addresses authorized to send emails for a particular domain.

Adding a TXT type record in the DNS record according to the SPF format will increase the credibility of the domain and prevent spam emails from spoofing the domain's sender to send spam emails.

<figure><img src="../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

The content in the red box is a typical SPF record, specifying the allowed domain names. The meaning of each field is as follows:

* `v=spf1` is the identifier for the SPF record, where `1` indicates the version of SPF. If using "Sender ID," this field should be `v=spf2`.
* `include:spf1.staff.mail.aliyun.com` specifies which third-party organizations are allowed to send emails on behalf of this domain.
* `-all` indicates that addresses not listed in the SPF record are **not authorized.**

## 0x02 DomainKeys Identified Mail (DKIM)

DKIM (DomainKeys Identified Mail) is an email authentication method designed **to detect forged sender addresses in emails.** It allows the recipient to verify that an email was indeed sent and authorized by the owner of that domain. This is achieved through cryptographic authentication.

DKIM adds an encrypted digital signature to each email, which is then **COMAPRED with records in a database of legitimate internet addresses**. Only emails with an encrypted signature that **matches the database records can enter the user's inbox**. DKIM also checks the integrity of the email, preventing unauthorized modifications by hackers and others.

Since the **digital signature cannot be forged**, DKIM can deal a fatal blow to spammers. It becomes difficult for them to achieve their goals by stealing the sender's name or altering attachment properties, as they did in the past.

To determine if DKIM is configured, you can check the email headers for the presence of a `DKIM-Signature`

<figure><img src="../.gitbook/assets/image (22).png" alt=""><figcaption><p>CSIT Lunar new year Mobile Challenge Email</p></figcaption></figure>

### Why DKIM cant be forged?

DKIM (DomainKeys Identified Mail) is difficult to forge due to its use of cryptographic techniques involving private and public keys.

1. **Public-Private Key Cryptography**:
   * **Private Key**: The private key is known only to the domain owner and is used to create the digital signature for outgoing emails.
   * **Public Key**: The public key is published in the domain’s DNS records and is used by the receiving mail server to verify the digital signature.
2. **Digital Signature**:
   * When an email is sent, the mail server uses the private key to generate a digital signature. This signature is unique to the email's content and headers.
   * The signature is then attached to the email as part of the `DKIM-Signature` header.
3. **Verification Process**:
   * Upon receiving the email, the recipient’s mail server retrieves the public key from the DNS records.
   * The server uses this public key to decrypt the digital signature and compare it with a newly computed hash of the received email’s content and headers.
   * If the decrypted signature matches the computed hash, the email is verified as authentic and untampered.

## 0x03 DMARC

DMARC (Domain-based Message Authentication, Reporting, and Conformance) is an email authentication protocol that builds on SPF (Sender Policy Framework) and DKIM (DomainKeys Identified Mail) to provide a higher level of protection against email spoofing and phishing. DMARC allows domain owners to specify policies for handling emails that fail SPF or DKIM checks and provides mechanisms for reporting email authentication results back to the domain owner.

#### How DMARC Works

1. **DNS Record**: The domain owner publishes a DMARC record in the DNS. This record specifies the domain's DMARC policy, which includes how to handle emails that fail SPF or DKIM checks and where to send reports.
2. **Email Sending and Receiving**: When an email is sent, the recipient's mail server checks the DMARC record of the sender's domain.
3. **Policy Enforcement**: Based on the DMARC policy, the recipient's server decides how to handle emails that fail authentication. The policy can specify whether to reject, quarantine, or accept such emails.
4. **Reporting**: The recipient's server sends aggregate and/or forensic reports back to the domain owner, detailing authentication results and any actions taken.

\
To configure DMARC, you need to add a TXT record with the prefix `_dmarc` to your domain.

<figure><img src="../.gitbook/assets/image (23).png" alt=""><figcaption><p>TaoBao dmarc</p></figcaption></figure>

* **`v=DMARC1`**: Specifies the version of DMARC being used (DMARC version 1).
* **`p=quarantine`**: Specifies the policy for handling emails that fail DMARC checks. In this case, the policy is to quarantine (mark as spam) such emails.
* **`rua=mailto:dmarc-tb@service.alibaba.com`**: Specifies the email address where aggregate reports should be sent. These reports provide summary information about the DMARC validation results for emails sent from the domain.

## Interview Question

1. What is SPF and how does it help in preventing email spoofing?
2. Explain how DKIM works and why its digital signature is difficult to forge
3. What role does DMARC play in email authentication and how does it enhance protection against email spoofing?
4. What is the difference if email failes the DMARC checks and the policy is reject or quarantine?

## Author

[Ik0nw](https://github.com/Ik0nw)

## Reference

[https://cloud.tencent.com/developer/article/2401022](https://cloud.tencent.com/developer/article/2401022)
