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
  - a container for records that define how to route traffic to a domain and its subdomains
  - you will pay 50 cents per month per hosted zone
    #### Public Hosted Zones
      - contains records that specify how to route traffic on the Internet (public domain names)
    #### Private Hosted Zones
      - contains records that specify how you route traffic within one or more VPCs (private domain names)

  ### TTL (Time To Live)
  - the client will cache the result of a hostname for the duration of the TTL , so if you change the IP address before the TTL i up, it will still show the old IP address until time is up,then change to the update IP
  - Do this so we dont query the DNS server (route 53) too much
  - High TTLs - less traffic to Route 53 but possible outdaed information
  - Low TTLs - more traffic on Route 53 (more $$), records not as outdated, easy to change records
  - TTL is mandatory except for Alias records
 
  ### CNAME vs Alias
   - if you want to map a hostname to another hostname:
     ##### CNAME
     - points a hostname to any other hostname
     - only works if you have a non-root domain name

     ##### Alias
       - points a hostname to an AWS resource
       - works for root domains and non root domains (mydomain.com)
       - free of charge
       - native health check ability
         ###### Alias Records
           - an extension of DNA functionality
           - automatically recgonizes change in the resources IP address
           - unlike CNAME, it can be used for the top node of a DNA namespase (Zone Apex) ex. example.com
           - will always be Type A or AAAA
           - You cant set the TTL
           - Record Targets:
             1. ELBs
             2. Cloudfront Distributions
             3. API Gateway
             4. Elastic Beanstalk Env
             5. s# websites
             6. APC interface endpoints
             7. Global Accelerator
             8. Route 53 record in the same hosted zone
             9. NOT for an ECS DNS name

    ### Routing Policies
    - defines how Route 53 respont o DNA queries
    - routing is not like the loadbalancer, it doesnt route any traffic. It only responds to DNS queries. DNS just helps traslate hostnames, no routing
      #### Health Checks
        - check health of mainly public resources
        - have automated failover
          1. health checks that monitor an endpoint (app, server, AWS resource)
             - abt 15 global health checks will check the endpoint health
             - you can set a threshold for healthy or unhealthy (3 is the default). This is how many times does it have to fail for it to be unhealthy from one health checker
             - you can set an interval 30 sec, smaller interval of 10 sec costs more $$$
             - HTTP, HTTPS TCP
             - if more than 18% of checkers say its healthy then Route 53 deems it so
             - you get to choose which regions the checkers are in
             - health checks pass only when the endpoint responds with the 2xx or 3xx status codes
             - health checks can be setup to  pass/ fail based on the text in the first 5120 bytes of the response
             - configure your route/firewall to allow incoming requests from route 53 health checkers
          3. health checks that monitor other health checks
             - combine the results of multiple health chekcs into a single check
             - you can use OR AND NOT
             - can monitor up to 256 child health checks
             - specify how many health checks need to pass to make the parent pass
             - usage: perform maintenance to your website without causing all health checks to fail
          5. health checks that monitor a CW alarm
          6. health checks are integrated with CW metrics
        ##### Health Checks Private Hosted Zones
        - route 53 health checkkers are outside the VPC so it will be difficult. they cant access private endpoints
        - so instead you have to create a CW metric and associte the CW alarm, then create a health check that checks the alarm itself
          
    - Route 53 Suppotys the following routing policies:
      #### Simple Routing
        - typically route traffic to a single resource
        - can specify multiple values in the same record (multiple IP addresses)
        - if multiple values are returned, a random one is chosen by the client
        - when alias is enabled, you can only specify one AWS resource
        - cant be associated with health checks
     
      #### Weighted
        - control the % of the requests that go to each specfic resource
        - assign each record a relative weight
        - weights dont need to sum to 100%
        - DNS records must have the same name and type
        - health checks 
        - use cases: load balancing between regions, testing new app versions
        - assigna weight of 0 to a record to stop sending trafic to a resource
        - if all records have a weight of 0, then all records will be returned equally
     
      #### Latency-based
        - redirect to a resource that has the least latecny close to us
        - super helpful whenlatency for users is a priority
        - latency is based ontraffic between users and AWS regions
        - Germany users may be directed to the US (if thats the lowest latency)
        - health checks
        - when making these recors you have to specify where the IP address you are routing to is located. AWS isnt smart enough yet to know which regions IP adresses are from
     
      #### Failover (Active-Passive)
        - when a health check is associated with an instane and it is unhealthy, Route 53 will failover to the seconday instance that is the disaster recovery instance
        - you will have a primary and seconday instance for your record
     
      #### Geolocation
        - based on where the user is actualy located
        - specify by continet, country, or by US state (if there is overlapping, most precise location selected)
        - should create a "Default" record ( in case there's no match on location)
        - use cases: website localization, restrict content distribution, load balancing
        - health checks
     
      #### Geoproximity
        - route traffic to your resources based on the geographic locaton of users and resources
        - ability to shift more traffic to resources based on the difined bias
        - to change the size of the geographic region, specify bias values
          - expand : (1 - 99) more traffic to the resource
          - shrink : (-1 - -99 less traffic to the resource
        - resources can be:
          - AWS resources (specify AWs region)
          - non-AWS resources (specify longitutude and latitude)
        - you must use route 53 traffic flow (advanced) to use this feature
        - if bias is set to 0, lines will be split evenly based on that region proximity
        - helpful when you need to increase bias to one region
     
      #### IP-Based
        - routing based on clients IP
        - in route 53 you provide a list of CIDRs (pernounced cider) for your clients and the corresponding endpoints/locations
        - use cases: optimize performance, reduce network costs
        - ex. route end users from a particular ISP to a specfic endpoint
     
      #### Multi-Value
        - when you want to route traffic to multiple recources
        - route 53 will return multiple values/resources
        - health checks
        - up to 8 healthy records are returned for each multivalue query
        - multivalue is not a substitube for having an ELB

    ### Domain Registar vs. DNS Service
    - you buy or register your domain name with a Domain Registar typically by paying annual charges (GoDaddy, Amazon Registar Inc, etc.
    - the registar usually provides you with a DNS service to manage your DNA records (Route 53 hosted zone)
    - but you can use another DNS service to manage your DNS records
    - do if you decide to buy a domain name from godaddy, you will need to create a hosted zone in route 53 then  you will need to change the nameservers (NS) on godaddy to the name servers from ROute 53 to use Route 53 as your DNS service to manage your DNS records

    ### Hybrid DNS
    - if you want to resolve DNS queries between your VPC (Route 53 resolver) and on-site premises (other DNS resolvers)
    - you have to use a resolver endpoint on your VPC. The on-premises data center will connect via a VPN and then connect to the inbound or outbound endpoint
    - networkds can be:
      - VPC itself / Peered VPC
      - on-premises network (connected through Direct Connect or AWS VPN)
     
      #### Resolver Endpoints
        - inbound endpoint : allows your DNS resolvers to resolve domain names for AWS resources and records in Private Hosted Zones
        - outbound endpoint : route 53 forwards DNS queries to your DNS resolvers












  
  
