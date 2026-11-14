SOC Lab 2 — Splunk Cloud Security Incident Investigation

Project Overview

This project demonstrates a simulated Security Operations Center (SOC) Tier 1 investigation using Splunk Cloud.

I analyzed simulated Windows Security Event Logs to identify suspicious authentication activity, investigate repeated failed login attempts, correlate successful logins, and assess possible unauthorized access across multiple systems.

The purpose of this lab was to practice the responsibilities of an entry-level SOC Analyst, including log analysis, threat detection, event correlation, incident triage, and escalation recommendations.

Disclaimer: All security events, usernames, hostnames, and IP addresses in this investigation are part of a simulated training environment. No real attack or confirmed compromise occurred.

⸻

Tools and Technologies

* Splunk Cloud — Security Information and Event Management (SIEM)
* Splunk Search Processing Language (SPL)
* Windows Security Event Logs
* CSV log ingestion
* GitHub — Technical documentation and portfolio

Skills Practiced

* Security event monitoring
* SIEM investigation
* Windows authentication log analysis
* Failed and successful login correlation
* Suspicious IP address investigation
* Potential brute-force attack identification
* Potential lateral movement investigation
* Incident documentation
* SOC Tier 1 triage and escalation

⸻

Lab Environment

SIEM Platform: Splunk Cloud

Data Source: Simulated Windows Security Logs

Dataset: SOC_Lab_2_Simulated_Login_Logs.csv

Splunk Index: main

Sourcetype: csv

Environment: Simulated enterprise network

The dataset contained Windows authentication events associated with multiple user accounts and computer systems.

I uploaded the CSV dataset into Splunk Cloud and used SPL searches to investigate authentication patterns.

Windows Event IDs Investigated

Event ID 4625 — Failed Logon

Indicates an unsuccessful authentication attempt.

Event ID 4624 — Successful Logon

Indicates that an authentication attempt succeeded.

Both event types were important for determining whether repeated failed login attempts were followed by successful authentication.

⸻

Investigation 1 — Suspicious Failed Login Activity

Objective

Identify accounts experiencing repeated failed authentication attempts and determine whether the activity could indicate password guessing or a brute-force attack.

Splunk Search

index=main username="jsmith"

Findings

I identified suspicious authentication activity involving the account jsmith.

The simulated logs showed:

* Seven consecutive failed login attempts between approximately 2:01 PM and 2:07 PM.
* A successful login at approximately 2:08 PM.
* The same source IP address associated with the activity.
* Authentication events targeting JFS-DC01.

Source IP: 198.51.100.24

Username: jsmith

Target System: JFS-DC01

Analysis

The repeated failed authentication attempts followed by a successful login created a suspicious pattern.

This behavior may indicate password guessing followed by successful authentication.

However, authentication logs alone do not establish that an attacker gained access. Additional evidence would be required to distinguish malicious activity from legitimate user behavior.

Assessment: Suspicious authentication activity requiring further investigation.

⸻

Investigation 2 — Successful Authentication and Potential Lateral Movement

Objective

Determine whether the suspicious account authenticated to another system following the initial successful login.

Splunk Search

index=main extracted_host="JFS-WKS07"

Findings

I identified successful authentication records associated with:

Username: jsmith

Source IP: 198.51.100.24

Destination System: JFS-WKS07

Event ID: 4624

Time: Approximately 2:09 PM

The logs showed authentication activity on JFS-WKS07 shortly after a successful login to JFS-DC01.

Two matching records were visible for the workstation. Because their displayed fields were identical, I could not establish whether they represented separate logons or duplicate records.

Analysis

Successful authentication to another computer shortly after suspicious login activity may indicate lateral movement.

However, the available evidence does not prove that lateral movement occurred.

Additional endpoint activity, authentication details, and unique event identifiers would be needed to determine whether the activity was authorized.

Assessment: Potential lateral movement requiring additional investigation.

⸻

Investigation 3 — Comparing Authentication Activity Across Accounts

Objective

Compare the suspicious activity involving jsmith against another account to understand differences in authentication patterns.

Splunk Search

index=main username="rpatel"

Findings

The account rpatel showed:

* Four failed login events.
* Three successful login events.
* Source IP address 10.0.0.21.
* Authentication activity involving JFS-DC01.

Additional Search

index=main username="rpatel" action="success"

This search isolated successful authentication events for the account.

Analysis

Unlike jsmith, the rpatel account showed intermittent successful and failed authentication attempts.

This pattern could represent normal user activity, authentication difficulties, or suspicious behavior.

The available logs did not provide enough information to determine the cause.

Assessment: Requires context before classifying the activity as legitimate or malicious.

⸻

Investigation 4 — Source IP Address Correlation

Objective

Determine which authentication events originated from specific IP addresses.

Splunk Search

index=main src_ip="10.0.0.21"

Findings

The IP address 10.0.0.21 was associated with authentication activity involving rpatel.

The investigation also identified 198.51.100.24 as the source IP associated with suspicious jsmith authentication activity.

Analysis

Source IP correlation helped identify relationships between accounts, authentication attempts, and destination systems.

The address 198.51.100.24 belongs to a reserved documentation range and was used only for this simulated investigation.

It should not be interpreted as a verified malicious public IP address.

⸻

Incident Timeline

Time	Event	System
2:01–2:07 PM	Seven failed jsmith logins	JFS-DC01
2:08 PM	Successful jsmith login	JFS-DC01
2:09 PM	Successful jsmith authentication records	JFS-WKS07

Date: November 14, 2026

Timeline Analysis

The sequence of events raised concerns because multiple failed logins were followed by successful authentication and subsequent activity on another system.

This pattern justified further investigation into possible credential compromise and lateral movement.

⸻

SOC Tier 1 Incident Assessment

Incident Category: Suspicious Authentication Activity

Potential Threat: Password Guessing / Credential Compromise

Additional Concern: Possible Lateral Movement

Affected Account: jsmith

Systems of Interest:

* JFS-DC01
* JFS-WKS07

Source IP: 198.51.100.24 (simulated)

Recommended Priority: Medium, pending organizational severity criteria and additional evidence.

Evidence Supporting Escalation

1. Multiple consecutive failed authentication attempts.
2. Successful authentication immediately following the failures.
3. Consistent source IP address.
4. Successful authentication activity involving a second system.
5. Insufficient evidence to confirm whether the activity was authorized.

Investigation Limitations

The simulated dataset provided authentication records but did not include enough context to confirm account compromise, identify a real attacker, or prove lateral movement.

The investigation also could not determine whether identical authentication records represented separate events or duplicates.

⸻

Recommended Incident Escalation

If this activity were detected in a production SOC environment, I would recommend escalating the findings for additional investigation.

Recommended Next Steps

1. Verify whether the authentication activity was authorized by the account owner.
2. Review additional Windows Security Logs and endpoint telemetry.
3. Investigate the source and context of the authentication attempts.
4. Examine authentication activity on both affected systems.
5. Check for unusual processes, remote sessions, or additional account activity.
6. Determine whether the account requires containment according to organizational procedures.
7. Document findings and escalate to the appropriate incident response team.

Escalation Decision: Escalate for further investigation due to the suspicious authentication sequence and possible cross-system activity.

⸻

Key Takeaways

This lab helped me understand how SOC Analysts investigate suspicious authentication events using a SIEM platform.

I gained hands-on experience searching Windows Security Logs, identifying unusual login patterns, correlating events across systems, and evaluating potential security incidents.

One important lesson was that suspicious activity does not automatically confirm a security breach.

SOC Analysts must evaluate evidence carefully, document their findings, recognize investigation limitations, and escalate incidents when necessary.

⸻

Project Evidence

Screenshots of the Splunk searches and investigation results will be included in this repository to demonstrate the analysis process.

All screenshots are from a simulated training environment.

⸻

Career Development

This project is part of my ongoing cybersecurity portfolio as I develop practical skills in security monitoring, incident investigation, and SOC operations.

Career Interests: SOC Analyst, Security Analyst, Cybersecurity Analyst, and Incident Response.

Certification: CompTIA Security+

Lab 1: Wireshark Network Traffic Analysis

Lab 2: Splunk Cloud Security Incident Investigation