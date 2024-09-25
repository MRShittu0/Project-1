# Active Directory Project

## Objective

The goal of this project is to create a functional Active Directory home lab environment, with integration to Splunk for monitoring and analysis. This lab will simulate real-world network and security scenarios, allowing for hands-on experience with AD management, user authentication, Group Policy Objects (GPOs), and security auditing. Splunk will be utilized to collect, monitor, and analyze AD logs, providing insights into user activity, system events, and potential security threats within the lab environment.

### Skills Learned

- Configuring and managing AD Domain Controllers
- Managing users, groups, and organizational units (OUs)
- Setting up DNS and DHCP services for a domain
- Auditing Active Directory logs for security and authentication events using Splunk
- Identifying and analyzing potential security incidents such as failed login attempts and successful attempts
- Installing and configuring Splunk Universal Forwarder on AD servers
- Ingesting and indexing AD logs into Splunk for real-time analysis
- Configuring a virtual lab environment with multiple VMs for AD, DNS, and Splunk

### Tools Used

- **VirtualBox**: For creating virtual machines to host the AD Domain Controller, Splunk server, and other components.
- **Windows 2022 Server**: For setting up and managing the Active Directory Domain Controller, DNS, and DHCP.
- **Windows 10**: Used as the SOC Analyst computer and it has splunk universal forwarder, AtomicRedTeam, and Sysmon
- **Ubuntu Server**: Used as Splunk for centralizing, monitoring, and analyzing AD logs.
- **Kali Linux**: Used as the attacker machine to brute force the AD server.
- **Sysmon**: Used to collect telemetry on the server to send to Splunk.
- **Splunk Universal Forwarder**: For collecting and forwarding logs from AD servers to Splunk.
- **PowerShell**: For automating AD tasks, log forwarding, and system management.
- **NAT**: Used for all the VMs to be under the same network.
- **AtomicRedTeam**

## Steps
**Create a Network Diagram**
- Created a Logical Diagram on [draw.io](url) mapping out how the home lab works logically. This will help show how data will flow.
![image](https://github.com/user-attachments/assets/6b3cc388-73cb-4f59-9ff3-6e365fff2015)


**Download and Install Virtualbox**
- Download Oracle Virtualbox 
![Screenshot 2024-09-23 103702](https://github.com/user-attachments/assets/908b89d9-451b-4038-96d2-6e5a088bc735)
- You will need to install Microsoft Visual C++ redistribution package first before installing Virtualbox.
![image](https://github.com/user-attachments/assets/74426e10-74f8-4948-bc3a-db079f34a954)
- Once Vitualbox is installed, you can go ahead and create the NAT address Clicking on Tools >> Network >> Select NAT Networks and create one to add the VMs to them.
![Screenshot 2024-09-22 185141](https://github.com/user-attachments/assets/d54b5f4c-40a8-4d2d-b2bf-3dcf78b8570a)


 **Download & Install Windows 10 (SOC Analyst Machine)**
 - Download windows 10 ISO file to install windows 10 on the Virtualbox.
![image](https://github.com/user-attachments/assets/b06606b8-00d1-4648-bfa1-ab5de190086a)


**Download & Install Windows Server (Active Directory)**
- Download: Obtain a Windows Server ISO from Microsoft’s website.
![image](https://github.com/user-attachments/assets/2f4a3712-fa8a-40cd-928e-5bca225dcf6c)
- Install: Set up the server on the virtualbox by clicking on 'New' and filling the information then add the ISO image. Click on "Skip Unattended Installation" to avoid errors.

**Configure AD:**
- Promote the server to a Domain Controller using the Active Directory Domain Services (AD DS) role.
- Install DNS as part of the AD setup.
- Create domain users, groups, and Organizational Units (OUs).

**2. Download & Install Splunk**
- Download: Get Splunk Enterprise(ubuntu server 22.04) from Splunk's website (free trial available).
- Install: Install Splunk on the Virtualbox.
- During installation, choose default settings for simplicity.
- After installation, log in to Splunk Web and complete the setup.
![Screenshot 2024-09-22 191032](https://github.com/user-attachments/assets/25b8a1e0-1890-4dd5-9278-0dd5ad608b57)
- Configure Splunk IP addresses to the above using sudo nano /etc/netplan/50-cloud-init.yaml
- After inputing the address you run "sudo netplan apply" to apply the settings
- Run sudo apt-get unstall virtualbox-guest-additions-iso, then sudo reboot

**Install Splunk Universal Forwarder:**
- Download and install the Universal Forwarder on the AD server to collect AD logs.
- Configure forwarder to send security event logs and Sysmon logs to the Splunk server.

**3. Download & Configure Sysmon for Enhanced Logging**
- Download Sysmon from the Microsoft Sysinternals website.
- Install Sysmon on the AD server using a configuration file Olafhartong's config on GitHub.
- Forward Sysmon logs to Splunk by configuring the Splunk Universal Forwarder.

**4. Set Up Kali Linux for Brute Force Testing**
- Download: Get Kali Linux ISO from the official website.
- Install: Set up Kali Linux in the virtualbox.
- Tools to Use: Install crowbar to perform brute-force attacks.
**Brute Forcing AD:**
- Used crowbar to brute force AD credentials using RDP service.
- Ensure to configure AD with weaker test accounts for simulation.
![Screenshot 2024-09-22 141001](https://github.com/user-attachments/assets/bcb4ad62-a79b-44d7-bbc9-cc3cc95d4777)


**5. Install and Configure Atomic Red Team for Attack Simulation**
- Download: Clone the Atomic Red Team repository from GitHub.
- Install: Run Atomic Red Team on a separate machine or the AD server itself.
- Usage: Use Atomic Red Team to execute various MITRE ATT&CK techniques (e.g., lateral movement, privilege escalation, persistence).
- Execute atomic tests to simulate attacks on AD (e.g., credential dumping, process injection).
- Monitor and detect these attacks in Splunk using AD event logs and Sysmon logs.

**Final Steps: Monitor and Analyze Logs**
- Splunk Dashboards & Alerts:
- Set up dashboards in Splunk to visualize AD authentication events, failed logins, privilege escalations, and Sysmon activity.
- Configure alerts in Splunk to notify you of brute force attempts or atomic test activities.
- Brute Force Detection: Use Splunk to monitor for brute force attacks initiated from Kali Linux (e.g., failed logins or locked accounts).
- Threat Detection: Monitor for Sysmon events triggered by Atomic Red Team simulations.

This setup provides a robust learning platform for AD management, security monitoring with Splunk, attack simulation with Atomic Red Team, and brute force testing using Kali Linux.
