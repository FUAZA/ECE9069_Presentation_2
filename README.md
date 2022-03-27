# CVE-2021-28040 OSSEC-HIDS

## Outlines
- Background of OSSEC-HIDS
- The issue in CVE-2021-28040
- What is XML and Uncontrolled Recursion
- Related Attack Patterns
- Impacts
- Potential Mitigations

## Background of OSSEC-HIDS
OSSEC-HIDS is a host-based intrusion detection system that is free and open-source, which offers four main features, including log analysis, integrity checking, rootkit detection, and active response. Meanwhile, OSSEC has a central manager which is in charge of monitoring and receiving data from agents, syslog, databases, and agentless devices. It stores the file integrity checking databases, the logs, events, and system auditing entries.

The agents will gather information and forward it to the manager for analysis and correlation. 
![image](https://www.ossec.net/docs/_images/ossec-arch.jpg)

This diagram above shows the central manager receiving events from the agents and system logs from remote devices. When certain conditions are detected, active responses can be performed and the administrator is notified.

## The issue in CVE-2021-28040
![image](https://user-images.githubusercontent.com/101413304/160301804-b5ff864d-9a2d-4837-9035-9a927a42075d.png)

The figure above shows the information on CVSS Scores & Vulnerability Types on the version of  OSSEC 3.6.0. Since the entire risk level ranges from 0-10, we can see that the CVSS score is 5, which is the median risk of vulnerability. When a high number of opening and closing XML tags are used, an uncontrolled recursion vulnerability in the os_xml.c code arises, as described in CVE-2021-28040. Because the os_xml.c file allows for undefinable recursion cycles. As a result, once unmapped memory is reached, an attacker can cause a segmentation fault.

[The image is from here](https://www.cvedetails.com/cve/CVE-2021-28040/)

## What is XML
The Extensible Markup Language (XML) is a text-based format for encoding structured data, such as documents, data, configuration, books, transactions, invoices, and more. It's also popular in sharing structured data between programmes, computers, and humans, both locally and over networks.

Many computer systems have data in formats that are incompatible. Web developers spend a lot of time transferring data between incompatible (or upgraded) systems. Incompatible data is frequently lost when large amounts of data must be translated. Data is stored in plain text format in XML. This provides a method of storing, transmitting, and sharing data that is independent of software and hardware. Data can be made available to a variety of "reading machines," such as people, computers, voice machines, news feeds, and so on, using XML.

Here is the example of XML from OSSEC

![image](https://user-images.githubusercontent.com/101413304/160302424-fc9c1a87-cda0-454b-b915-8b863a5c2f42.png)

[More Information about XML](https://www.w3schools.com/xml/xml_whatis.asp)
## What is Uncontrolled Recursion

### Uncontrolled Recursion Example

![image](https://user-images.githubusercontent.com/101413304/160302663-45d50319-ec82-43d2-8ea3-0f86fd71f7e3.png)
![image](https://user-images.githubusercontent.com/101413304/160302643-29e27128-5a95-4424-90d8-6434c811d5d5.png)

[The images are from here](https://en.wikipedia.org/wiki/Stack_overflow)

## Related Attack Patterns

[More Information about XDoS](https://www.security-database.com/capec.php?name=CAPEC-82)
[More Information about XML Parser Attack](https://www.security-database.com/capec.php?name=CAPEC-99)

## Impacts

## Potential Mitigations

## Reference
- https://www.cvedetails.com/cve/CVE-2021-28040/
- https://cwe.mitre.org/data/definitions/674.html
- https://github.com/ossec/ossec-hids/issues/1953
- https://www.w3schools.com/xml/xml_whatis.asp
- https://www.security-database.com/cwe.php?name=CWE-674
- https://www.security-database.com/capec.php?name=CAPEC-82
- https://www.security-database.com/capec.php?name=CAPEC-99
- https://en.wikipedia.org/wiki/Stack_overflow
