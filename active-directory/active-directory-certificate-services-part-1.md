# Active Directory Certificate Services Part 1

ADCS is a commonly used service in windows Server environments that provides public key infrastructure (PKI) functionality to create, manage, and store certificates for various security and encryptions uses, including communications and digital signatures

E.g.

* Smart cards
* SSL Certificates
* Code signing
* etc....

## How it works

Clients send certificate signing requests (CSRs) to a certificate authority(CA), which signs issued certificates using the private key for the CA certificates

## Certificate Enrollment

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption><p>Certficate Enrollment</p></figcaption></figure>

**1) Key Pair Generation**: As the client, you start by generating a public/private key pair. The private key is securely stored on your device, while the public key is included in the Certificate Signing Request (CSR).

**2) Sending the CSR**: You submit the CSR to an Enterprise Certificate Authority (CA). This request contains your public key and additional identifying details. It is **signed with your private key** to authenticate the origin of the request.

**3) CA Validation**: Upon receiving the CSR, the CA conducts several verifications:

* **Existence of the Certificate Template**: The CA checks whether the requested certificate template exists.
* **Compliance with Template Settings**: The CA reviews if the settings specified in the CSR comply with those allowed by the certificate template.
* **User Enrollment Permissions**: The CA confirms if you, as the requester, have the necessary permissions to enroll for a certificate as defined by the template’s policies.

**4) Certificate Generation**: If all validations are successful, the CA generates the certificate, detailing its purpose (e.g., code signing) and including the requester’s information. This step integrates your public key into the certificate.

**5) Certificate Signing and Issuance**: The CA signs the certificate using its private key to establish its authenticity, then sends it back to you. This signature ensures that any recipient of the certificate can trust its source and validity.

**6) Client Reception and Utilization**: After receiving the certificate, you store it in the Windows Certificate Store on your device. You can then use the certificate for various permitted actions, such as authentication and code signing, according to the Extended Key Usage (EKU) specified in the certificate.

## Certificate Templates

CAs issues certificates with **blueprint** settings defined by certificate templates (Stored as AD objects)

<figure><img src="../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

## How to trust CA?

### Internal Enterprise CA

In order to validate and trust a Certificate Authority (CA), the CA should be integrated as a **part of the Active Directory domain.** This integration ensures that the CA's root certificate is recognized and trusted across all domain members, allowing for seamless authentication and secure communication within the network.

### External CA

If using an external or third-party CA, the root certificate of the CA needs to be manually distributed and trusted by adding it to the Trusted Root Certification Authorities store on all domain members. This can be done via **Group Policy in an AD environment**, ensuring all clients and servers within the domain trust certificates issued by the external CA.

### Security consideration

While integrating a CA with AD, it's important to consider security implications:

* **Only trusted CAs** should be added to the domain’s trust store, as any certificate issued by these CAs will be trusted for various operations within the domain.
* Properly configure certificate templates and CA permissions to avoid unauthorized issuance of certificates.

## NTAuthCertificates

When store ADCS, this object called NTAuthCertificates is created inside active directory schema and contains field called CA Certficates and it contains list of CA that are **trusted for authentication**&#x20;

<figure><img src="../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

## What is the process of validation of user certificate?

When you, as a client, hold a certificate and attempt to authenticate to a service in an Active Directory (AD) environment (for instance, for smart card logon, securing remote desktop sessions, or other forms of certificate-based authentication), the process includes the following steps:

1. **Certificate Presentation**: You present your certificate to the service or system you are trying to authenticate with. This could be a domain controller, a web server, or any other service that uses certificate-based authentication.
2. **Certificate Validation**: The service or system checks the validity of your certificate. This involves several checks, including:
   * **Validity Dates**: Ensuring the certificate is neither expired nor not yet valid.
   * **Revocation Status**: Checking if the certificate has been revoked.
   * **Trustworthiness**: Verifying that the certificate was issued by a Certificate Authority (CA) that is trusted by the system.
3. **Trust Check Against NTAuthCertificates Store**: For certain types of authentication like smart card logon:
   * The system verifies that the CA which issued your certificate is listed in the NTAuthCertificates store in Active Directory. This store contains the certificates of CAs that are trusted to issue certificates for the purposes of Windows authentication.
   * If the issuing CA’s certificate is found in the NTAuthCertificates store, it signifies that the CA is trusted to issue authentication certificates within the domain. If the CA is not listed, the authentication process will fail because the system cannot establish trust in the presented certificate.
4. **Authentication Decision**: If all the checks are passed (including the presence of the issuing CA’s certificate in the NTAuthCertificates store for required scenarios), the system proceeds to authenticate you based on the credentials provided (i.e., the certificate and possibly additional factors like a PIN if using a smart card).

## Author:

[Ikonw](https://github.com/Ik0nw)

## Interview question

1\) Can you explain what happens when a certificate is presented for authentication in an Active Directory environment?

2\) Where does NTAuthCertificate store? In AD domain controller or AD certficate services

3\) What is the prerequiste of Internal trust for CA

\
Reference
---------

[https://www.youtube.com/watch?v=Gg9rJm2GMBY](https://www.youtube.com/watch?v=Gg9rJm2GMBY)
