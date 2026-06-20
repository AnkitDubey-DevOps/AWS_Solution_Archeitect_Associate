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

## Migrating DNS to Amazon Route 53

If you are currently using another DNS Service and you want to migrate to Amazon Route 53

→ Start by creating a Hosted Zone.

Route 53 automatically assigns the delegation set, the four name servers, to your Hosted Zone.

→ To ensure that the DNS queries for your domain go to the Route 53 name servers,

Update your registrar's or your DNS service's NS records for the domain to replace the current name servers with the names of the four Route 53 name servers for your Hosted Zone.

The method that you use to update the NS records depends on which registrar or DNS service you are using.

→ Some registrars only allow you to specify name servers using IP addresses; they don't allow you to specify Fully Qualified Domain Names (FQDNs).

If your registrar requires using IP addresses, you can get the IP addresses for your name servers using the dig utility (for Mac/Linux) and nslookup (for Windows).

## Transferring a Domain Between Accounts Within AWS
Transferring a domain to a different AWS account

If you registered a domain using one AWS account and you want to transfer the domain to another AWS account, you can do so by contacting the AWS Support Center and requesting the transfer.

### Migrating a hosted zone to a different AWS account
If you are using Route 53 as the DNS service for the domain, Route 53 does not transfer the hosted zone when you transfer a domain to a different AWS account.

If domain registration is associated with one account and the corresponding hosted zone is associated with another account, neither domain registration nor DNS functionality is affected.

The only effect is that you will need to sign in to the Route 53 console using one account to see the domain, and sign in using another account to see the hosted zone.

## Supported DNS Record Types by Route 53

1. A Record (Address Record) : Maps a hostname/domain name to an IPv4 address.
2. AAAA Record: Maps a hostname/domain name to an IPv6 address.
3. CNAME Record (Canonical Name) : Creates an alias from one domain/subdomain to another hostname. eg : (web.example.com → www.example.com)
4. NS Record (Name Server) : Specifies the authoritative name servers for a domain. Used for DNS delegation.
5. SOA Record (Start of Authority) : Contains administrative information about the DNS zone.
6. MX Record (Mail Exchange) : Specifies the mail server responsible for receiving emails for a domain. Lower preference value = higher priority.

### NS Records
NS records define which name server is authoritative to a particular zone or domain name and point you to other DNS servers.

#### A / AAAA Records
A/AAAA are called host records, like business cards.

#### CNAME Record
CNAME is an alternative record or alias for another record.

Helpful in redirection or if you want to hide details about your actual servers from the users.

## SOA (Start of Authority) Record
### What is an SOA Record?
Every DNS hosted zone contains exactly one SOA (Start of Authority) record.

It is automatically created when a hosted zone is created.

It appears at the beginning of the DNS zone data.

Information Stored in an SOA Record

1. Domain Owner Information : Contact email address of the domain administrator.
2. Authoritative Name Server : Primary DNS server responsible for the zone.
3. Serial Number : Version number of the zone. Incremented whenever DNS zone data changes.
4. Refresh Interval : How often secondary DNS servers check for updates.
5. Retry Interval : Time before retrying if a refresh attempt fails.
6. Expire Time : Time after which secondary servers stop serving the zone if updates cannot be obtained.
7. TTL (Time To Live) : Default caching duration for DNS records.
