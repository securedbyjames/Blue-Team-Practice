<h1>Blue Team Practice</h1>

<h2>Overview</h2>
Blue Team Practice documents my hands-on journey toward developing practical Security Operations Center (SOC) skills. I am actively completing the SOC Level 1 learning path on TryHackMe alongside CyberDefenders Blue Team labs to build a well-rounded foundation in security monitoring, incident investigation, and threat detection.

TryHackMe provides structured learning around core SOC concepts such as log analysis, SIEM usage, threat detection, and security fundamentals. CyberDefenders complements this by offering realistic investigation scenarios that simulate real-world security incidents and blue team workflows.

Throughout this repository, I document labs from both platforms, including my investigation process, tools used, queries written, findings, and lessons learned. The goal is to demonstrate practical SOC analyst skills, reinforce my learning through documentation, and build a portfolio of hands-on security investigations.

<h2>TryHackMe Labs</h2>

<a href="https://github.com/securedbyjames/Blue-Team-Practice/blob/main/TryHackMe/Benign.md">Benign Lab Walkthrough</a>

  - <b>Desription:</b> I investigated Windows process execution logs (Event ID 4688) ingested into Splunk after an IDS alert indicated a potential compromise within the HR department. Using Splunk searches and log analysis techniques, I identified a suspicious user account impersonating a legitimate employee, detected the use of scheduled tasks, and traced malicious activity involving the LOLBIN certutil.exe being used to download a payload from an external file-sharing site.


<h2>CyberDefenders Labs</h2>

<a href="CyberDefenders/WebStrike Lab.md">WebStrike Lab</a>

  - <b>Desription:</b> Analyzing network traffic using Wireshark to investigate a web server compromise, identify web shell deployment, reverse shell communication, and data exfiltration.

