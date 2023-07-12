---
layout: default
title: WAF
parent: Security Protection
grand_parent: Public Cloud
permalink: /public-cloud/security-protection/web-application-firewall/
nav_order: 1
---
# UWAF - Web App Firewall UWAF

SCloud Web Application Firewall (UWAF) is a cloud-based distributed reverse proxy application firewall that uses the cloud with more resources and higher throughput to identify and filter traffic. In the increasingly severe network security situation, UWAF, which has features such as high availability, efficient interception of malicious traffic, access statistical analysis, application health monitoring, etc., and flexible management through the console or APIs, is an ideal solution for web security for enterprise users in various industries.

UWAF protects your web applications by blocking most web attacks that reduce application availability, security, or cause abnormal consumption utilization. It differs from traditional application firewalls in that you can pay as you go, open as you go, fast access, and convenient management. You can also combine UWAF as part of your network security solution with other security products such as Anti-DDoS Pro for more comprehensive security. In addition, UWAF can meet the requirements for equal protection.

### Suitable for users
All customers who require Web Application Guard (applications or origin servers can not be deployed on SCloud).
### Access method
- UWAF for SaaS: Deployments through CNAME resolution, domain name resolution directs traffic to the CNAME-protected domain name assigned by UWAF.
- UWAF for ULB: After purchase, add forwarding rules on the ULB side, and then add domain names on UWAF to perform web security detection on traffic without changing the original network link.
### Basic product functionality
- Protection against general web attacks (SQL injection, XSS attacks, etc.).
- Mainstream web vulnerability detection and protection, the latest high-risk vulnerability protection, virtual patches.
- CC (Challenge Collapsar) attack detection and prevention.
- Flexible custom protection policies.
- Various access and attack reports.

## Feature description
SCloud Web Application Firewall (UWAF) is used to protect against internal and external security threats against web applications. Compatible with Anti-DDoS Pro (UDDoS), it can protect against network attacks such as injection, command execution, and CC that are common on the network in addition to the protection provided by Anti-DDoS Pro service.

### Basic concepts 
Enterprise-level web application firewall is deployed before the origin server, after purchase, the cname provided by ufaf is configured into the cname resolution of the domain name, and then all public network traffic will pass through ufaf, malicious attack traffic will be filtered, and normal traffic can be transmitted to the origin server through ufaf, thereby ensuring the security of the origin server.

### Web Application Attack Protection 
Comprehensive protection against the following attack types: SQL injection, XSS cross-site, WebShell, command injection, illegal HTTP protocol requests, common web server exploits, unauthorized access to core files, path traversal, etc. Provide functions such as backdoor isolation protection and scanning protection.

### Fine-grained access control rules
Provides a friendly configuration console interface, supports the combination of conditions of common HTTP fields such as IP, URL, Referer, and User-Agent, creates a powerful precise access control policy, and supports protection scenarios such as hotlinking and website background protection.

It adopts linkage mechanism with security modules such as common web attack protection and CC protection to create a multi-layer comprehensive protection mechanism that can identify trusted and malicious traffic according to requirements.

### Malicious CC attack protection
Control the access frequency of single-source IP, support redirect verification, and human identity.

In response to massive slow request attacks, comprehensive protection is carried out based on statistical response codes, URL request distribution, abnormal Referer and User-Agent feature recognition, and precise website access control.
### Enrich reports
Provides rich attack and access reports to keep you informed of website status.
### Black and white lists
- Blacklist: Block specified IP addresses or IP segments.
- Whitelist: Allows the specified IP address or IP range.
### Log query and download
UWAF supports real-time query of 10,000 log queries online within 3 days. Provides log downloads (attack logs and access logs) for 7 days.

If you enable the Log Extension Pack service, you can download logs for up to 180 days (free for users of the Ultimate version or above).

### Alarm management
Regularly send alert summary information of UWAF-related domain names to user mailboxes to remind you of risks.

Send alerts from UWAF-related domain names that trigger a large number of rules in a short period of time, and send alarms to users' mailboxes or mobile phones to remind them of the risk of security incidents.

Send an alert that the origin server of the UWAF domain name is not responding normally in real time to the user's mailbox to remind the user to check the origin server.

Send UWAF-related domain name request response status in real time, such as the proportion of status codes above 499 in the overall request is greater than 30%. An alert will be sent to the user's email or mobile phone.

## Product advantages
### Low latency is more stable
Based on BGP line access, UWAF provides multi-line node disaster recovery and intelligent optimal path intelligent mobilization, with stable quality, millisecond-level response, and low latency.
### Rules self-perfecting system
At present, mainstream WAFs are still based on feature matching, but in the face of increasingly complex and diverse attacks, the rule system based on feature matching often cannot be well protected. UWAF can also be based on machine learning, using the excellent generalization ability and automatic learning ability of the intelligent detection engine, which can be organically combined with the rule system to provide a more solid guarantee for the security of the customer's website.
### Automated elastic scalability
In the face of business uncertainty, UWAF has automatic elastic scaling capabilities, and uses SCloud's powerful public cloud resource pool as a support, which can quickly expand its own services in the event of CC attacks or business emergencies, so there will be no performance bottleneck.
### Powerful collaborative protection capabilities
SCloud leverages powerful cloud-based intelligence gathering capabilities, combined with intelligence from other intelligence vendors, to filter massive malicious access requests on UWAF with an intelligence base.
### 24/7 expert service
Anyone who purchases UWAF can enjoy 24/7 free expert service. For some complex attacks, when the machine or algorithm cannot accurately judge and block, SCloud's security experts can intervene and assist, and make targeted protection measures for the attack scenario according to the hacker's attack methods.

## Overdue recycling
### Alarm
The billing system sends alarm notifications to customers.
Account managers manually submit reclaimed resources: Alerts customers within 10 minutes.
### Automatically reclaimed resources:
Monthly and annual billing: an alarm is issued once in the morning on day 1, once on day 3, and once on day 6;
Hourly billing: One alarm for the next 10 points.
### Renewal
If you renew the subscription within 7 days after the alarm is sent, you can continue to use it normally and keep the original configuration.
### Recycling policy
If the alert is not renewed after it is issued, the resource will be deleted 7 days after the first alarm is issued.
### User deletion
If the user deletes the WAF during the use period, the equivalent fee for the elapsed time of the current month will be charged, and the remaining fee will be returned to the user's account.
## Version selection
If you are an enterprise user with certain requirements for disaster recovery and traffic, we recommend that you choose UWAF for the Enterprise Edition. If the service traffic is greater than `100 Mbps`, we recommend that you select the appropriate UWAF version based on the size and importance of your service.

The trial version and premium version are offline, and the historical user configuration is retained and can continue to be used. For test drive users who have purchased a test drive for more than 3 months, configuration editing is restricted, that is, configurations cannot be added and modified, and user configurations that have already taken effect will not be affected.

### Version comparison

> `×` indicates that the feature is not supported in this version ULB stands for this item depending on the configuration of the ULB.

#### Function description

| Product parameters | Description | Enterprise Edition | Flag ship version | Exclusive customized version | ULB Zone Edition |
| --- | --- | --- | --- | --- | --- |
| HTTP | HTTP (80) port security | Supported | Supported | Supported | ULB |
| HTTPS | HTTPS (443) port security | Supported | Supported | Supported | ULB |
| HTTP2.0 | HTTP2.0 service forwarding and security protection | Supported | Supported | Supported | ULB |
| Non-standard ports | Security for business ports other than port 80,443 | Supported | Supported | Supported | ULB |
| Origin server outside the cloud | The user application/origin server is deployed outside the SCloud cloud | Supported | Supported | Supported | × |
| Foundation defense | XSS, SQL injection, command execution, and other common web attacks | Supported | Supported | Supported | Supported |
| Wildcard domains | Add wildcard domain name protection | Supported | Supported | Supported | Supported |
| TLS configuration | You can configure global (all domain names) TLS versions and cipher suites | Supported | Supported | Supported | ULB |
| Bandwidth expansion | Increase bandwidth through extension packages outside the version limit | Supported | Supported | Supported | ULB |
| Domain extension | Expand the package outside the version limit to add more domains | Supported | Supported | Supported | Supported |
| Exclusive IP | Increase the number of exclusive IP points through the expansion package outside the version limit | Supported | Supported | Supported | × |
| Log expansion package | Logs are retained for 180 days through the extension package, which is eligible for equal protection | Supported | Supported | Supported | Supported |
| Self-defined defense | Configure custom protection rules for multiple conditions | Supported | Supported | Supported | Supported |
| 0Day Defense | Quickly protect against the latest web vulnerabilities | Supported | Supported | Supported | Supported |
| CC Defense | CC offensive defense, tacit or custom defense strategy | Supported | Supported | Supported | Supported |
| CC blocks IPs | View or unblock IP addresses blocked by CC rules | Supported | Supported | Supported | × |
| Malicious IP bans | Block IPs that trigger protection rules multiple times | Supported | Supported | Supported | Supported |
| Regional IP blocking | Implement access control for specific regions based on rules | Supported | Supported | Supported | Supported |
| Information security protection | Corresponding anti-protection filtering is carried out according to the rules | Supported | Supported | Supported | Supported |
| IP lookups | Query the access and attack of the domain name by the IP address | Supported | Supported | Supported | Supported |
| Black and white list | Add IPs, IP blocks to block access to specific IPs | Supported | Supported | Supported | Supported |
| Log service | Search query and download of real-time logs | Supported | Supported | Supported | Supported |
| Certificate management | Adding, deleting, and binding SSL certificates | Supported | Supported | Supported | ULB |
| Intercept the page | Alert or block page after a custom trigger rule | × | Supported | Supported | × |
| Web page anti-usurpation | To a certain extent, prevent the web page from being usurped | Supported | Supported | Supported | × |
| Security alerts | Send security risk or service exception alerts via SMS or email | Supported | Supported | Supported | Supported |

*Illustrate:*
For more information about the availability zones supported by ULB Private Zone, see Price description.
Non-standard ports are supported except for ports below `80`.

#### Performance and quota comparison: 

| Product parameters | Enterprise Edition | Flag ship version | Exclusive customized version | ULB Zone Edition |
| --- | --- | --- | --- | --- |
| Bandwidth (out-of-cloud/in-cloud). | 40Mbps/120Mbps 
| 60Mbps/200Mbps 
| 100Mbps/300Mbps | ULB |
| Total number of domain names (pcs). | 20 | 50 | 70 | 20 |
| Number of wildcard domain names | 2 | 5 | 7 | 2 |
| Exclusive IP Number of points (pcs). | 3 | 5 | 10 | × |
| Domain name deployment region(s). | 1 | 2 | 3 | ULB location |
| QPS | 3000 | 5000 | 10000 | ULB |
| System rules (domain names/articles). | 20 | 40 | 50 | 20 |
| CC rules (domain names/bars). | 10 | 20 | 30 | 10 |
| CC Guard Peak Value (QPS). | 50000 | 100000 | 300000 | ULB |
| Malicious IP bans (domains/articles). | 5 | 5 | 5 | 5 |
| Regional IP block (domain/article). | 10 | 10 | 10 | 10 |
| Information security protection (domain name/article). | 10 | 20 | 30 | 10 |
| IP queries (bars/day). | 30 | 30 | 30 | 30 |
| Black/white list (domain name/article). | 500 | 1000 | 3000 | 500 |
| Global black/white list | 10 | 10 | 10 | 10 |
| Log query and download | Supported | Supported | Supported | Supported |
| Logs are stored for 180 days | × | Supported | Supported | × |
| Web page anti-usurpation (domain name/article). | 20 | 20 | 20 | × |
| Wildcard domains | Supported | Supported | Supported | × |
| Intercept the page | × | Supported | Supported | × |
| Customized needs | × | Supported | Supported | Supported |

Illustrate: ULB Zone Edition: ULB Zone Edition WAF needs to bind at least one ULB, bind multiple ULBs for cumulative billing, and the version quota will also accumulate.

For example, binding 2 ULBs requires 7,300 RMB/month, supporting a total of 40 domain names, and other quotas will be doubled.

Total number of domain names/number of wildcard domain names: You can add 1 wildcard domain name for every 10 domain names, that is, under normal circumstances, you can only add 2 wildcard domains for the Enterprise Edition, and so on for other versions.

CC protection peak: The CC protection peak in the table is the value obtained by the experimental environment test, and the actual protection peak is related to the network environment and the number of new connections.

Malicious IP block*: If the attack type of a rule is All, only one rule can be set and no other attack types can be added. If you have rules with a specific attack type, you cannot add rules with an attack type of All.

If the version quota does not meet your needs, you can purchase the expansion pack, click Extension Pack for details

#### Parameter description

| Parameter | Illustrate |
| --- | --- |
| Bandwidth (out-of-cloud/in-cloud). | The origin/app is deployed within the SCloud cloud (such as a UHost host) and is actually deployed in the same region as UWAF , enjoy the bandwidth in the cloud, and use the bandwidth threshold limit in the cloud, and in other cases, the bandwidth threshold limit outside the cloud is used to return to the source of the public network. If the user's service bandwidth exceeds the version limit, the delay may increase and the service chain may increase Risk of disconnection |
| Number of domain names | The maximum number of domain names that can be added to the version, 1 wildcard domain name can be added for every 10 domain name quotas, and the domain name quota can be increased by purchasing expansion packages |
| Exclusive IP score | An exclusive IP credit can be tied to a WAF protection IP for a domain name. Compared to domains that share EIPs, domains with dedicated IPs do not affect other domains when trafficked. You can earn more points by purchasing expansion packs |
| The region where the domain name is deployed | The domain name configuration generates a working area, and it is recommended to select a region similar to the origin server to reduce access latency |
| QPS | Version supports the maximum back-to-origin QPS, which is the number of rings per second and represents the maximum throughput. If this limit is exceeded, there may be a risk of increased delay and disconnection of the business chain |
| System rules | Customize rules based on fields such as IP, User-Agent, Referer, Request Method, and Request Content , each field can be selected with logical conditions such as contains, greater than, regular, and multi-row matching |
| CC rules | Block or pop up captcha requests based on the frequency of requests for a file or path from the source IP IP with high frequency |
| CC guard peak value | If the maximum number of concurrent connections in the current version exceeds this limit, the risk of delay may increase and the service chain may be broken |
| Malicious IP bans | Block IPs that repeatedly trigger protection rules, and trigger malicious IP blocking rules The IP will be added to the blacklist |
| Regional IP blocking | According to the origin of the request, determine whether to block or release the request |
| Information security protection | Sensitive information such as mobile phone numbers and identity cards is desensitized to prevent leakage, and at the same time, it can also be blocked according to the echo content, and the format of the echoing content must be text/html or text/plain. You can also disguise the response content according to the echo code of the origin station, or change the response to block |
| IP lookups | The IP query function can query the basic information of a specified IP address and access WAF within the specified time band Please request statistical information |
| Black and white list | Requests for IPs or IP segments in the blacklist will be blocked, and IPs or IP segments in the whitelist will be blocked Please be allowed to go without security checks |
| Log query downloaded | By default, you can query the last 10,000 attack and access logs within 3 days, or you can download them within 7 days Please ask for logs and attack logs. For longer log storage and downloading, purchase the Log Expansion package (Users above the flag ship version are provided with 180 days of log storage for free). |
| Logs are stored for 180 days | Attack logs and access logs are stored for 180 days, and users can download attacks for the last 180 days in the console or access logs |
| Web page anti-usurpation | Add HTML/htm static** page to prevent it from being usurped by hackers |
| Wildcard domains | Add wildcard domain name protection |
| Intercept the page | Alarm blocking section content of custom trigger rules, supporting HTML and TXT formats |
| Customized needs | If some functions cannot be adjusted in the console, please consult technical support |

