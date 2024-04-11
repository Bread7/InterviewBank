# Indicators of Compromise

Decided to share a bit about blue team stuff since there isn't much blue team stuff in this interview bank. I decided to leverage on my experience as a SOC Analyst ahahaha. Indicators of Compromise is a must-know for EVERY blue teamer.&#x20;

**What are Indicators of Compromise (IOC)?**

An Indicator of Compromise (IOC) is a piece of digital forensics that suggests that an endpoint or network may have been breached. So yes, IOCs are evidences of potential intrusion on a host system or network. It allows Infosec professionals and system administrators to detect intrusion attempts or other malicious activities. Many security researchers use IOCs to better analyze a particular malware's techniques and behaviors. IOCs are also used to provide actionable threat intelligence (also known as Cyber Threat Intelligence) that can be shared within the community to further improve an organization's incident response and remediation strategies. \
\
Some IOCs are found on event logs and timestamped entries in the system. Security professionals often employ the use of a blacklist to monitor for IOCs to help mitigate and prevent breaches or attacks.&#x20;

**Examples of IOCs**

* Unusual traffic going in and out of the network
* Unknown files, applications, and processes in the system
* Suspicious activity in administrator or privileged accounts
* Irregular out-of-the-ordinary traffic.
* Anomalous spikes of requests and read volume in company files.
* Network traffic that traverses in unusually used ports.
* Tampered file, DNS and registry configurations as well as changes in system settings
* Large amounts of compressed files and data unexplainable found in locations where they shouldn't be.

**Type of IOCs**

There are different types of IOCs&#x20;

* File-based Indicators - These are usually associated with a specific file. These types of indicators would usually come in a hash format (MD5, SHA1, SHA256)
* Network-based Indicators -  There are indicators associated with a network and they would come in the form of an IP address or domain name.
* Behavioural Indicators - Some indicators are associated with the behaviour of a system or network, such as unusual network traffic or unusual system activity. You can view the [MITRE ATT\&CK framework](https://attack.mitre.org/) to look at some of these examples.&#x20;
* Artefact-Based Indicators - These are indicators associated with the artefacts left behind by an attacker, such as a registry key or a configuration file.

**Interview Questions**

1\) Define the term "Indicator of Compromise" (IOCs) and explain how they are used in incident response.

2\) What is the difference between Indicators of Compromise (IOCs) and Indicators of Attack (IOAs)?

3\) Do you think having Indicators of Compromise (IOCs) is enough? Explain why.

## Author

- [Isaac](https://github.com/frostsg)

