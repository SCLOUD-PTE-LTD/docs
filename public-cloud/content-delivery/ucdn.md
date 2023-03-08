---
layout: default
title: UCDN
parent: Content Delivery
grand_parent: Public Cloud
permalink: /public-cloud/content-delivery/ucdn/
nav_order: 1
---

# UCDN (SCloud Content Delivery Network)
## Overview
UCDN (SCloud Content Delivery Network) service, or Cloud Content Delivery Network, is a distributed content delivery network built on a network of multiple vendors. Relying on edge servers deployed in various places, through the load balancing, content distribution, scheduling and other functional modules of the central platform, users can obtain the required content nearby, reduce network congestion, improve user access response speed and hit rate, greatly increase the number of concurrent access, and avoid the adverse impact caused by single point failure.
## Features and benefits
SCloud Content Acceleration System is a completely self-developed content acceleration software system, which is an important part of SCloud's public cloud and has been operating online for more than five years. At present, more than 250 self-built nodes are deployed, 500+ converged nodes are global, the operating bandwidth is close to `6.5Tbps` and more than 300 customers are served. 

The system supports the acceleration of static files such as on-demand, downloading, and web pages, as well as dynamic acceleration of live broadcasts.
- All-in-one solution: Backed by the SCloud cloud platform, it provides integrated solutions such as data collection, processing, storage, and distribution.
- Flexible and customizable: Self-developed cache system, not dependent on the limitations of open source systems, can flexibly support personalized needs, and provide customized services for customer needs.
- Security Acceleration Guarantee: Provide user security through anti-hotlink or IP restriction policies.

## Key concepts
### Accelerated domain name
The user needs an accelerated domain name, which is usually owned by the customer.
### CDN domain name
When the acceleration domain name passes the review, the system generates a CDN domain name, and you need to use this domain name to modify the corresponding CNAME record to realize intelligent location selection and scheduling of CDN. For more information about how to modify it, see "DNS Configuration CNAME Record".
### The IP address or domain name of the origin server
The site where the user actually provides services (which can be SCloud or a third party) provides services to the public network by IP or domain name.
### Content refresh
Content refresh refers to deleting a specified directory or file from an acceleration node, so that users request to return to the source to obtain it, and you can use this manual cache refresh method when you want users to see the latest information in time.
### Prefetch files
Prefetching simulates the user's first request on all acceleration nodes, allowing specific content to be cached in CDN nodes, improving the user's first download experience and reducing back-to-origin traffic.

| Function | Page acceleration | Large file download acceleration/VOD acceleration|
| --- | --- | --- |
| Domestic acceleration | ✓ | ✓ |
| Acceleration abroad | ✓ | ✓ |
| Content Renewal | ✓ | ✓ |
| Prefetch files | ✓ | ✓ |
| Slow storage configuration | ✓ | ✓ |
| Operational logs | ✓ | ✓ |

Among them, the biggest difference between web page acceleration and large file acceleration or video-on-demand acceleration is mainly determined by the attributes of the file to be accelerated:

- If the resources to be accelerated are mainly static small files such as pictures and documents, then web page acceleration is preferred, web page acceleration is refreshed and prefetched once a day by default, and small file files are updated quickly, and manual refresh and prefetching by people are inefficient, which wastes labor costs.
- If the resources to be accelerated are mainly large files or installation packages of more than 30 M, large files are prioritized for acceleration.
- If the resources to be accelerated are video-based, VOD acceleration is preferred.

### Page acceleration
Page acceleration can distribute the static content of the customer's website, such as web pages, images, text, etc., to server nodes around the world, so it can significantly improve the response speed and usability of the website. In addition, for security reasons, page acceleration also provides anti-hotlink function to protect website content from being intercepted.

This acceleration type is available for portals, social networking sites, e-commerce sites, government sites, enterprise portals, and news media sites.
### Large file download acceleration
Large file acceleration can accelerate video files, game installation packages, software, patches and other content that needs to be downloaded, providing customers with download acceleration solutions. You only need to upload large files to the nearest server node, and the UCDN platform can intelligently deploy files and distribute them to nodes across the country, not only solving the problem of insufficient bandwidth but also allowing your users to experience faster download services.

Large file acceleration is suitable for the gaming industry, software download websites, distance education, book application providers, and more.

### Video on demand acceleration
VOD acceleration is used to accelerate video files, first upload the content of the origin server to the data center, and then distribute it to edge servers across the country through internal private protocols, combined with streaming media technology to transmit to end users. So as to improve the user's viewing experience, improve the problem of slow access and viewing lag.

VOD acceleration is suitable for streaming service websites such as online video websites and distance education MOOCs.