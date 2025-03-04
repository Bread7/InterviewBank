# Secure C2 Infrastructure

The following high-level diagram illustrates a secure C2 (Command and Control) infrastructure:

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption><p>Resource from CRTO2</p></figcaption></figure>

## **Key Components:**

* **Public Cloud:**
  * **Expendable Redirectors:** Forward HTTPS traffic from the victim network to the C2 server via an SSH tunnel.
* **Victim Network:**
  * Sends encrypted C2 traffic using HTTPS to the redirectors.
* **Red Team On-Premise:**
  * **Simulated Adversary:**
    * **C2 Server:** Core server where operators control the compromised systems.
    * **Operators:** Individuals managing and executing the commands via the C2 server.

**Operational Workflow:**

1. **Victim Network to Redirector:**
   * The victim network sends HTTPS traffic to the public-facing redirector. This traffic is typically encrypted and may be further obfuscated using techniques like domain fronting.
2. **Redirector to C2 Server:**
   * The redirector forwards the HTTPS traffic through an SSH (or VPN) tunnel to the C2 server. This tunnel ensures that the communication remains encrypted and secure.
3. **C2 Server Processing:**
   * The C2 server processes the commands received through the secure tunnel and responds accordingly. The responses are sent back through the same tunnel to the redirector, which then forwards them back to the victim network.

**Redirector Role:**

* The redirector's main function is to handle and redirect HTTPS traffic coming from the victim network. It acts as an intermediary, forwarding this traffic to the actual C2 server. The redirector does not hold any sensitive information or perform any complex operations; it simply forwards the traffic.

## **Importance of Tunnel Direction:**

1. **No Direct Inbound Access:**
   * By setting up the tunnel from the C2 server to the redirector, the C2 server does not need to expose itself directly to the internet. This prevents direct inbound access to the C2 server, enhancing its security and reducing the risk of detection.
2. **Protection of Sensitive Information:**
   * Since the tunnel is established and maintained by the C2 server, sensitive private keys and credentials remain on the C2 server. They are not exposed to the redirector, which could be less secure or potentially compromised.

## **Benefits of Using Redirectors:**

* **Enhanced Operational Security (OpSec):** Hides the real C2 server's location and reduces the risk of exposure.
* **Load Balancing and Redundancy:** Distributes traffic across multiple redirectors, ensuring high availability and fault tolerance.
* **Evasion of Detection:** Frequently changing IPs and **domains** make it harder for defenders to block and track the C2 infrastructure.

## Author

Ik0nw

## Interview

1\) Do redirector have to be a instance or vm?

2\) Why important credentials or keys cannot store in redirectors

3\) Why C2 Server cannot be expose to public?

4\) Why using HTTPS is a common type of egress traffic? Is there other options for opsec?

