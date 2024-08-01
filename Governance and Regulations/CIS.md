CIS (Centre for internet security) is a non-profit entity whose aim is to set a global standard and best practices for  securing IT systems.

The CIS controls and benchmarks are constantly reviewed by experts in the field and modified based on recent trends on cyber attacks.

`CIS is divided into three areas: Basic, Foundational, Organisation.`

#### Basic:
- ﻿﻿Inventory and Control of Software Assets
- ﻿﻿Inventory and Control of Hardware Assets
- ﻿﻿Continuous Vulnerability Management
- ﻿﻿Controlled use of administrative privileges
- ﻿﻿Secure configuration for hardware and software on mobile devices, laptops, workstations and servers
- ﻿﻿Maintenance, monitoring and analysis of audit logs

#### Foundational:
- ﻿﻿Email and web browser protection
- ﻿﻿Malware defences
- ﻿﻿Limitation and control of network ports, protocols and services
- ﻿﻿Data recovery capabilities
- ﻿﻿Secure configuration for network devices, such as firewalls, routers and switches
- ﻿﻿Boundary defence
- ﻿﻿Data protection
- ﻿﻿Controlled access based on need to know
- ﻿﻿Wireless access control
- ﻿﻿Account monitoring and control

#### Organisation:
- ﻿﻿Implement a security awareness and training program
- ﻿﻿Application software security
- ﻿﻿Incident response and management
- ﻿﻿Penetration test and red team exercises



==Let's elaborate through each of them:==

1. `Inventory and control of hardware assets`: Attackers might possibly be waiting for new or unprotected systems to be added on our network, there might be different types of devices come and go off through the network, policies such as BYOD(Bring your own device) will allow employees to bring there on devices, and might be out of synchronisation with security updates or may be already compromised. Even a single miss of update will be a jackpot for attacker. Also the devices which are not visible from outside(internet) should also be taken care of. Devices with possibly vulnerable softwares and all should be isolated from the network and should be immediately replaced. Always keep in mind that attackers are closely monitoring your infrastructure and patiently waiting for an opportunity.There should be managed control of all devices as it is an important step in incident response, recovery, backup etc. Keep in mind that managing and control of devices include setting up policies when devices compromised, lost etc.

2. `Inventory and control of Software Assets:` Just like hardware attackers might be eagerly waiting for a vulnerability on the software versions you run on your systems. Our softwares could easily be vulnerable if not updated regularly, also sophisticated adversaries can introduce a zero-day, or use a trustworthy third party to make these things possible. Often they try to install backdoors for a long and persistent exploitation. Poorly configured softwares are also an issue, the best way to prevent all these is to use an agent or a third part software or some sort of your own software to manage all the system's software, as you infrastructure grows as your organisation an manually doing stuff would be a task. By using such preventive measures, we can easily identify the compromised host and restrict the attacker from lateral movement and escalation of attack. similar to hardware control in software will help in incident response, backup, recovery etc.

3. `Continuous Vulnerability Management:` Every softwares we use or websites or applications have vulnerabilities that's not yet discovered, imagine a researcher submitting a vulnerability report, which is new, and eventually the details will be on CVE databases, attackers could leverage these vulnerabilities to make payloads for attack, there is always a rush b/w attackers , to use these information to carry out an attack and defenders to issue patch and take remediation as soon as possible.If defenders not participate in this rush gradually the attacker wins and this will lead to a massive cyber attack. So scanning the enterprise as whole, taking actions as soon as possible all are necessary here.

4. `Controlled Use of Administrative Privileges:`  Admin user have the topmost permissions on an infra. There will be database admins, It admins and so on.. improper management of these accounts will result in attackers to compromise hosts. The methods may be phishing, backdoors, key loggers and so on. So managing these accounts will be a crucial factor. 

5. `Secure Configuration for Hardware and Software on Mobile Devices, Laptops Workstations and Servers:` Manufacturers and distributers of these end-devices may not be configured their products in spite of security but ease of use. so configuring each device based on security will increase over all security posture of the org.

6. `Maintenance, Monitoring and Analysis of Audit Logs:` Maintaining logs and effectively monitoring and analysing them is very important as in most cases the only way to spot an attack might be using logs, if there are no logs we cannot find the attacker and find out may not even suspect an attacker this will be really harmful for an organisation. Using SIEM tools and configuring IDS and IPS will help in this.

7. `Email and Web Browser Protections:` As these endpoints are common source of interaction with users giving appropriate protection them is a crucial factor as phishing and other vulnerabilities can easily be induced here.

8. `Malware Defenses:` Anti malware softwares should be used as to be protected from malicious softwares as they can be easily used to compromise an entire org. so appropriate defences should be taken places in-order to prevent that.

9. `Limitation and Control of Network Ports, Protocols, and Services:` Many manufacturers do not configure systems based on security, many ports, services and protocols will be open. Attackers may use this loophole to pivot their attack. This should be taken care of seriously.

10. `Data Recovery Capabilities:` When attackers compromise machines, they might make significant changes to data stored. In this case we need efficient data recovery mechanisms to get back data. 

11. `Secure Configuration for Network Devices, such as Firewalls, Routers, and Switches:` As we know the manufacturers configure end devices for ease of use, and not for security. protocols, firewall and routers should be  configured appropriately for better security if not it will be easy for attacker to pivot into systems.

12. `Boundary Defence:` This is similar to last step where a boundary is set up as a layer defence to prevent attackers. Setting up rules for inbound and outbound traffic and so.

13. `Data Protection:` data protection is crucial. The best ways to protect stored datas are using encryption. Separate less sensitive from more sensitive as this will increase the security posture.

14. `Controlled Access Based on the Need to Know:` Only the information needed to the users should be visible to them. Access and permissions of sales person is not same as an HR. like ways maintain a hygeine on permission and access controls. 

15. `Wireless access control:` many attacks that already happened was due to wireless as it can easily be installed and stealthy, devices like flipper zero, wifi pineapple etc can be used to do this.

16. `Account monitoring and control:` Attacker might use previously used depricated accounts to take back access, some times when an employee leaves the organisation his account may be still active unwillingly, this will give the attacker access to sensitive data and also some time privilege escalation and lateral movement.

17. `Implement security awareness and trining programme:` The employees on an organisation may be unaware of the threat a cyber attack will present educating them and using campaigns like the phishing campaign will help in employees to understand about common attacks.

18. `Application software security:` This represents the vulnerabilities present in the web apps and native application like SQL injection, SSRF, buffer overflow etc. continously looking for vulnerabilities, conducting bug bounty campaigns, regular penetration tests and all will help in improving the application security.

19. `Incident Response and management:` logging in and monitoring is a crucial factor as told before, these logs will be analysed to find a security issues or event called incidents. Properly managing them, preparing incident response plans and all will help here.

20. `Penetration Tests and Red Team Exercises:` This will help in finding out vulnerabilities, poor configurations and so on on our systems. conducting red team engagements and all will be a crucial factor here.

All these will together help in improving security posture of the organisation, defining what are the improvements and configurations needed for this.