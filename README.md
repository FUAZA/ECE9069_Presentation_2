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
The Extensible Markup Language (XML) is a text-based format for encoding structured data, such as documents, data, configuration, books, transactions, invoices, and more. It's also popular in sharing structured data between programs, computers, and people, both locally and over networks.

Many computer systems have data in formats that are incompatible. Web developers spend a lot of time transferring data between incompatible (or upgraded) systems. Incompatible data is frequently lost when large amounts of data must be translated. Data is stored in plain text format in XML. This provides a method of storing, transmitting, and sharing data that is independent of software and hardware. Data can be made available to a variety of "reading machines," such as people, computers, voice machines, news feeds, and so on, using XML.

Here is the example of XML from OSSEC

![image](https://user-images.githubusercontent.com/101413304/160302424-fc9c1a87-cda0-454b-b915-8b863a5c2f42.png)

[More Information about XML](https://www.w3schools.com/xml/xml_whatis.asp)

## What is Uncontrolled Recursion
Uncontrolled Recursion is a vulnerability that occurs when a product fails to properly control the amount of recursion that occurs, causing it to use too much memory or the programme stack.

### Uncontrolled Recursion Example
Although both pow(base, exp) functions provide the same result, the one on the left is more likely to create a stack overflow since tail-call optimization is not allowed. The stack for these functions will appear as the bottom figures show during execution. Notice that the top-left function must hold an exponential number of integers in its stack, which will be multiplied when the recursion ends and the function returns 1. The function on the top-right, on the other hand, can only keep three numbers at a time and computes an intermediate result that is provided to the function's next iteration. A tail-recursion optimizer can "drop" the previous stack frames, avoiding the chance of a stack overflow, because no other information outside of the current function invocation must be retained.

![image](https://user-images.githubusercontent.com/101413304/160302663-45d50319-ec82-43d2-8ea3-0f86fd71f7e3.png)
![image](https://user-images.githubusercontent.com/101413304/160302643-29e27128-5a95-4424-90d8-6434c811d5d5.png)

[The images are from here](https://en.wikipedia.org/wiki/Stack_overflow)

## Related Attack Patterns
The uncontrolled recursion will result in two related attack patterns: XML Parser Attack and violating implicit assumptions regarding XML content (as known as XML Denial of Service).

Any technology that uses XML data can be subjected to XML Denial of Service. XDoS has three primary attack vectors that it can use. 
- Firstly , XDoS attacks use recursion to assault the CPU: the attacker produces a recursive payload and sends it to the service provider. 
- Secondly, jumbo payloads are used by XDoS to target memory: the service provider parses XML using DOM. DOM builds in-memory representations of XML documents, however when the document is too large, the service provider host may run out of memory trying to build memory objects. 
- Thirdly, XDoS attacks overload the system by flooding it with a large number of tiny files.

All of the above three attack vectors utilize the loosely coupled nature of web services, in which the service provider can hardly control the service requester and any messages sent by the service requester.

[More Information about XDoS](https://www.security-database.com/capec.php?name=CAPEC-82)

For XML parser attack, it occurs when an application utilizes an XML parser to convert user-controllable data, or when an application fails to provide adequate validation to verify that user-controllable data is safe for an XML parser.

[More Information about XML Parser Attack](https://www.security-database.com/capec.php?name=CAPEC-99)

## Impacts
This table lists the several specific impacts of the uncontrolled recursion. The Scope indicates the application security region that has been breached, while the Impact outlines the negative technical consequences that occurs if an adversary successfully exploit this vulnerability.
![image](https://user-images.githubusercontent.com/101413304/160305824-61eba904-a454-4389-9bcd-9950f567632f.png)

[The image is from here](https://cwe.mitre.org/data/definitions/674.html)

## Potential Mitigations

There are three possible mitigations. 
- The first is to increase the stack size, but this is only marginally successful. As a result, it may only be a temporary protection, as the stack is often still not very large, and attackers may still be able to trigger an out-of-stack problem.
- The second is to assure that an end condition is reached regardless of the logic conditions. Testing the depth of recursion and quitting with an error if the recursion goes too deep may be part of the end condition. The action's effectiveness depends on the complexity of the end condition. This approach has a moderate effectiveness.
- The third approach is the most effective mitigation for this issue. If the number of needed iterations is large, implement a code that regulates the number of permitted recursions, or adapt the methodology to loop over the XML tags using a while loop or for loop instead of recursing, resulting in stack exhaustion.

## Reference
- https://www.cvedetails.com/cve/CVE-2021-28040/
- https://cwe.mitre.org/data/definitions/674.html
- https://github.com/ossec/ossec-hids/issues/1953
- https://www.w3schools.com/xml/xml_whatis.asp
- https://www.security-database.com/cwe.php?name=CWE-674
- https://www.security-database.com/capec.php?name=CAPEC-82
- https://www.security-database.com/capec.php?name=CAPEC-99
- https://en.wikipedia.org/wiki/Stack_overflow
