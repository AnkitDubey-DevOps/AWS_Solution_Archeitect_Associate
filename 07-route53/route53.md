## Route53

You can use Route53 to register new domain, transfer existing domains, route traffic for your domains to your AWS and external resources, and monitor the health of your resources.

## DNS
Domain Name System which translates the human friendly hostnames into the machine IP addresses.

## Route 53 performs three main functions

1) Register a domain

2) As a DNS, it routes internet traffic to the resources for your domain.

3) Check the health of your resources.

Route 53 sends automated requests over the internet to a resource (can be a Webserver) to verify that the server is reachable, functional or available.

Also you can choose to receive notifications when a resource becomes unavailable and choose to route internet traffic away from unhealthy resources.

You can use Route 53 for any combination of these functions -

For eg. you can use Route 53 both to register your domain name and to route internet traffic for the domain.

or you can use Route 53 to route internet traffic for a domain that you registered with another domain Register.

## Domain Registration in Route 53

When you register a domain with Route 53:

Route 53 automatically becomes the DNS service for that domain.

It creates a Hosted Zone with the same name as the domain.

It assigns four Name Servers (NS records) to the hosted zone.

When users access your domain, these name servers direct browsers to the correct resources, such as:
Web Servers
Amazon S3 Buckets
Other AWS resources.

Route 53 obtains the Name Servers (NS records) from the hosted zone and associates them with the domain.

### AWS Supports
Generic Top-Level Domains (gTLDs)

Examples: .com, .org, .net

Geographic Top-Level Domains (ccTLDs / Geographic TLDs)

Examples: .in, .uk, .jp

### Registering a Domain with Route 53
A domain can be registered with Route 53 only if its Top-Level Domain (TLD) is present in the supported TLD list.

If the TLD is not supported by Route 53, the domain cannot be registered through Route 53.

### Using Route 53 as your Service
You can use Route 53 as the DNS Service for any domain, even if the TLD for the domain is not included on the Supported TLD list.

NOTE:
Each Amazon Route 53 account is limited to a maximum of 500 Hosted Zones and 10,000 resource record sets per hosted zone.

You can increase this limit by requesting to AWS.

### Delegate to Route 53
This step connects everything and makes it work.

Connect the domain name to the Route 53 hosted zone. This is called delegation.

Update your domain registrar with the correct name servers for your Route 53 hosted zone.

No other customer hosted zone will share this delegation set with you.

Doing this means Route 53 DNS service will be serving DNS traffic for the domain of the hosted zone.

If you registered your domain with a different registrar, you need to configure the Route 53 NS Servers list in your registrar DNS database for your domain.

## DNS Migration to Route 53

When switching an existing domain from one DNS provider to another:

The migration may take up to 48 hours to fully propagate.

During this period, some users may still be directed to the old DNS provider.

The delay occurs because NS (Name Server) records are cached throughout DNS servers worldwide.

This caching is controlled by the TTL (Time To Live) value.

Once the cache expires, DNS queries begin resolving through the new DNS provider (e.g., Route 53).

#### Key Point for Exams

DNS changes are not immediate. DNS propagation can take up to 48 hours due to cached NS records and TTL settings.

You can transfer a domain to Route 53 if the TLD is included on the following list.

If the TLD is not included, you can't transfer the domain to Route 53.

For most TLDs, you need to get an authorization code from the current registrar to transfer a domain.

## AWS Route 53 – Hosted Zone
### What is a Hosted Zone?
A Hosted Zone is a collection of DNS records for a specific domain.

It acts as a container that stores DNS configuration and routing information for a domain and its subdomains.

### How It Works
Create a Hosted Zone for a domain.

Add DNS records (A, CNAME, MX, etc.).

These records tell DNS how traffic should be routed for the domain.

#### Types of Hosted Zones
1. Public Hosted Zone

Used for Internet-facing domains.

Accessible from anywhere on the Internet.

2.Private Hosted Zone

Used for Internal DNS within a VPC.

Accessible only from associated VPCs.

Automatic Records Created

For every Public Hosted Zone, Route 53 automatically creates:

NS (Name Server) Record, 
SOA (Start of Authority) Record

Exam Point: Do not modify or delete the automatically created NS and SOA records unless specifically required.

→ Route 53 automatically creates a Name Server (NS) record with the same name as your hosted zone.

→ It lists the four name servers that are the authoritative name servers for your hosted zone.

→ Do not add, change, or delete name servers in this record.

→ When you create a hosted zone, Amazon Route 53 automatically creates a Name Server (NS) record and a Start of Authority (SOA) record for the zone.

→ The NS record identifies the four name servers that you give to your registrar or your DNS service so that DNS queries are routed to Route 53 name servers.

→ By default, Route 53 assigns a unique set of four name servers (known collectively as a Delegation Set) to each hosted zone that you create.

Example
ns-1337.awsdns-39.com
ns-895.awsdns-47.net
ns-428.awsdns-53.org
ns-1597.awsdns-07.co.uk

→ Once you update the Route 53 NS settings with your domain registrar to include the Route 53 name servers, Route 53 will be responsible to respond to DNS queries for the hosted zone.

This is true whether you do have a functioning website or not.

Route 53 will respond with information about the hosted zone whenever someone enters the associated domain name in a web browser.

→ You can create more than one hosted zone with the same name and add different records to each hosted zone.

Route 53 assigns four name servers to every hosted zone.

The name servers are different for each of them.

When you update your registrar's name server records, be careful to use the Route 53 name servers for the correct hosted zone — the one that contains the records that you want Route 53 to use when responding to queries for your domain.

Route 53 never returns values for records in other hosted zones that have the same name.

Hosted Zone → Default Records = NS + SOA

