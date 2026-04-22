# Vulnerability Scan with Nessus

### Introduction
This project demonstrates the end-to-end Vulnerability Management Lifecycle by securing a critical asset (Windows Server 2022 VM) using Nessus Vulnerability Scanner. This project simulates a real-world scenario where an outdated server is assessed, prioritized for risks using the CVSS framework, and systematically hardened through patching and configuration adjustments. The goal is to show how an organization can reduce its attack surface using proactive defense and security baselines effectively.

### Lab network diagram
<img width="584" height="342" alt="image" src="https://github.com/user-attachments/assets/9e58a760-1a71-4337-bc2f-fb5a381523d5" />


#### Technologies used
  Nessus, VMware Workstation 25H2, Windows Server 2022.

#### Key Concepts
  Nessus, Vulnerability Report, CVSS



### Step-by-step Implementation

#### Prerequisites

- Creating a service user account to be used by Nessus.
<img width="406" height="276" alt="image" src="https://github.com/user-attachments/assets/7b0cb17e-c98a-4f1c-a6e6-745253e37afb" />


#### Step 1: Create a New Advanced Scan

<img width="387" height="152" alt="image" src="https://github.com/user-attachments/assets/3fd9c708-c138-40fa-9f7e-a7f0bf7597b1" />

<img width="900" height="572" alt="image" src="https://github.com/user-attachments/assets/528f8fae-a531-4643-92e5-0faa16615f20" />


#### Step 2: Configure the scan's settings

##### 2.a) Set the device IP to be scanned

<img width="899" height="536" alt="image" src="https://github.com/user-attachments/assets/7ead5c59-8a9c-4a07-9e92-35bdd180df3e" />


##### 2.b) Set the credentials for the scan

This credential is needed for Nessus to be able to connect to the device and execute the scanning.
For this, a dedicated service account svc_nessus was created, following the best practice of using a credential created specifically for the scan rather than a real user’s credentials. This demonstrates the concept of Least Privilege: assigning specific permissions for scanning tasks rather than using a personal user account, which reduces the risk of credential theft.

<img width="1483" height="611" alt="image" src="https://github.com/user-attachments/assets/96f07128-4408-4042-ab9a-c8a218dd93a7" />

##### 2.c) After applying the configuration, run the scan.

For the plugin section, all plugins were selected to scan for all possible vulnerabilities.
<img width="693" height="714" alt="image" src="https://github.com/user-attachments/assets/021dfc5f-ea5a-4fce-9597-198ddbf959e5" />

<img width="1387" height="200" alt="image" src="https://github.com/user-attachments/assets/bf8f8fd2-62d5-4a5e-a5e0-5e0dd38c7491" />


#### Step 3: Vulnerabilities report

For each scan, Nessus generates a report with all the vulnerabilities found. In the first scan, 21 Critical, 43 High, 8 Medium, and 222 Info vulnerabilities were found.
<img width="1390" height="582" alt="image" src="https://github.com/user-attachments/assets/39619846-5692-4ce2-b119-5659b5ed024a" />

In the Vulnerabilities section, vulnerabilities are grouped depending on the software product they affect. There's a column count that shows how many vulnerabilities were found in each category. This is important to identify software that may have more vulnerabilities than others and prioritize patching.
<img width="1373" height="668" alt="image" src="https://github.com/user-attachments/assets/c5aa12b0-5018-46a5-8e3f-439bb89764ac" />

The Remediations section lists possible solutions for the vulnerabilities. Normally, these will be updates of already known vulnerabilities for programs or the OS. Each remediation shows how many vulnerabilities can be solved; this way, it can also be prioritized to implement one remediation that solves the most vulnerabilities.
<img width="1394" height="646" alt="image" src="https://github.com/user-attachments/assets/16eae6bf-310a-40e2-8f6d-cf5b05f7248f" />

Nessus offers the possibility to export the report in a PDF file, but in the Nessus Essentials version, this option is disabled:
<img width="642" height="317" alt="image" src="https://github.com/user-attachments/assets/74a31177-983f-42dd-bfe0-5b06bfd870dc" />


#### Step 4: Solving the vulnerabilities

##### 4.a) Updating the system to resolve most of the vulnerabilities.

Vulnerability Prioritization: Rather than fixing every alert, this project focuses on the Common Vulnerability Scoring System (CVSS). We prioritize Critical (9.0-10.0) and High (7.0-8.9) vulnerabilities, as these represent the most immediate risks to the enterprise network's availability.

For this testing lab, the scan was purposely done on a newly created Windows Server 2022 VM, so the scan would return many vulnerabilities. Most of these vulnerabilities are related to OS updates. Installing these OS updates first would resolve the majority of the weaknesses.

It's important to note that backups were made before updating the system. Best practices say that before any changes, measures should be taken in case a rollback is needed.

<img width="680" height="620" alt="image" src="https://github.com/user-attachments/assets/60fcef09-a4fa-4106-b918-268d8a9191bf" />

##### 4.b) Patching 

Another vulnerability analysis was done after installing the Windows OS updates.

In this case, only 4 High, 4 Medium vulnerabilities were found. The other 222 were information.

<img width="2186" height="928" alt="image" src="https://github.com/user-attachments/assets/625ceacd-7e37-42a6-8702-0e549ef979cd" />

<img width="2154" height="1015" alt="image" src="https://github.com/user-attachments/assets/6d61a848-8fec-42dc-8566-c1a5f0ea7a1a" />

As seen in the previous image, the majority of vulnerabilities were related to VMware Tools. The solution was to update this software. The other vulnerabilities were patched with some workarounds given by Nessus, for example, adding some registry keys that were needed in Windows.


#### Step 5: Final analysis

One last vulnerability scan was done after patching the OS and the programs.

The result only shows information about some services running on the server. These informations are important to take into account, because if the services had bad configurations, an attacker could compromise them.

<img width="2161" height="1083" alt="image" src="https://github.com/user-attachments/assets/cd01d065-be31-4f7d-9157-28aa9370a8e4" />

<img width="2166" height="1230" alt="image" src="https://github.com/user-attachments/assets/852c36ac-91a5-44f6-b164-3a148634b348" />
Scan results. No vulnerability was found.




### Lessons Learned

This project successfully demonstrated the Vulnerability Management Lifecycle: 
1. Discover (The initial scan)
2. Prioritize (Analyzing the High/Criticals)
3. Remediate (Applying patches/workarounds)
4. Verify (The final clean scan)

By focusing on the most CVSS critical vulnerabilities and the software with the most weaknesses, the attack surface could be reduced drastically faster, having a greater impact on decreasing business risk.

