# Route 53

## DNS
- Domain Name System - translate human friednly hostna,es into machine IP addresses
- DNS is backbone of the internet
- DNS uses heierarchical nameing structure
  - .com
  - example.com
  - www.example.com
  - api.example.com

   ### DNS Terminology
  - Domain Registrar
    - Amazon Route 53, GoDaddy
  - DNS Records
    - A, AAAA, CNAME, NS
  - Zone File : contains DNS records. How to match hostname to IP address
  - Name Server : resolves DNS quieres (Authooritative or Non-Authoritative)
  - Top Level Domain (TLD) : .com .us .in .gov .org
  - Second Level Domain (SLD) : amazon.com, google.com
  - Sub-Domain : www
  - FQDN ( Fully Qualified Domain Name ) : api.www.google.... (api)
  - HTTP : protocol

  ### How DNS Works
  1. If your local DNS server has never seen a website before, the local root server asks the root DNS server (managed by ICANN) steps in.
  2. If the Root DNS server doesnt know, it still has the domain (.com or .us etc) and then points to the, for example, .com TLD DNS server
  3. Then the TLD tells the Locol DNS to go to the SLD DNS Server (ex. example.com) which will be managed by a Domain Registrar
  4. Then the SLD DNA Server, send the Local DNS server the IP address for the website.
  5. The local DNS server then caches that answer so it knows next time
  6. Then the DNS server send the IP address your the client web browser
 
## Amazon Route 53
- a highly avalable, scaleable and fully managed and Authoritive DNS
  - authoritative : the customer (you) can update the DNS records
- is also a domain registrar
- Ability to check the health of your resources
- the only AWS service which provides 100% availiabilty SLA
- Route 53 name is a reference to the traditional DNS port used by DNS services

  ### Records
  - In Route 53 you will define many DNS records
  - the records define how you want to route traffic for a domain
  - each record contains:
    - domain/subdomain name
    - record type : A or AAAA
    - Value : 12.34.56.78
    - Routing Policy : how Route 53 responds to queries
    - TTL : amt of time the record cached at DNS resolvers
  - Route 53 supports the following DNS record types
    - must know: A AAAA CNAME NS
    - advanced : CAA DS MX NAPTR PTR SOA TXT SPF SRV

     #### Record Types
      - A : maps a hostname to IPv4 address
      - AAAA : maps a hostname to IPv6 address
      - CNAME : map a hostname to another hostname
        - the target is a domain name which must have an A or AAAA record
        - cant create a CNAME record for the top node of a DNS namespace (Zone Apex)
        - you cant create a CNAME for example.com but you can for www.example.com
      - NS : Name Servers for the Hosted Zone, they are the DNS names or IP addresses of the servers that respond to the DNS queries of your hosted zone
 
  ### Hosted Zones
  - a container for records that define how to route traddic to a domain and its subdomains
  - Public Hosted Zones : contains records that specify how to route traffic on the Internet (public domain names)
  
