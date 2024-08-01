Subdomain takeover is a security vulnerability that occurs when an attacker is able to take control of a subdomain of a website or a domain that is no longer in use by the organisation that originally set it up. This can happen when the DNS (Domain Name System) records for the subdomain still point to a service or resource that the organisation no longer controls.

Here's how subdomain takeover typically works:

1. An organisation sets up a subdomain (e.g., subdomain.example.com) and associates it with a specific service or resource, such as a cloud-based application, a content delivery network (CDN), or a third-party service.

2. At some point, the organisation decides to stop using the subdomain but fails to remove the DNS records pointing to it. This might happen because the organization forgets about the subdomain or doesn't properly manage its DNS configuration.

3. An attacker discovers the unused subdomain and finds that the DNS records still point to a resource or service they can control or exploit.

4. The attacker can then set up their own infrastructure or malicious content on the subdomain, effectively taking control of it.

5. When users visit the subdomain, they are directed to the attacker's infrastructure, where the attacker may engage in activities like phishing, spreading malware, or stealing sensitive information.

To prevent subdomain takeover, organisations should regularly review their DNS records and remove any references to subdomains that are no longer in use or needed. Additionally, they should follow best practices for managing their DNS configurations to ensure that they remain secure. Various tools and services can help identify and flag potentially vulnerable subdomains, allowing organisations to take action before attackers exploit them.