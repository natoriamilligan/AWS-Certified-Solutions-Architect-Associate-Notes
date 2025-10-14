# Load Balancers
## Scalability
  
  ### Vertical Scalability
  - increasing the size of an instance

  ### Horizontal Scalability (scale out or scale in)
  - increasing the number of instances
  - implies you have distributed systems

  ## Availability

  ### High Availability
  - usually goes hand in hand with horizontal scaling
  - running your app/instances for the same app in at least 2 data centers or two AZs
  - goal is to survive data loss
  - can be passive or active

  ## Load Balacing (Elastic Load Balancer)
  - servers that forward traffic to multiple servers (instances) downstream
  - the more users you have the more the load is going to be balacned accross instances but your users dont know what instances they are using.
  - expose a single point of access for your app (DNS)
  - handles dailures of downstream instances
  - can do health checks on instances
    - the ELB can provide the checks on the instance. If the instance isnt working, no traffic will be sent to it
  - provides ssl termination for your website
  - high avalabilty accros zones
  - separate public and private traffic
  - AWS guaratees that it will be working
  - AWs provides only a few configuration knobs
  - costs less than managing your own load balancer
  - ELB is intergrated with so many services in AWS
    ### Security Groups with ELB
    - users can access the balancer from anywhere
    - The EC2 instances only allow traffic from the ELB (security group to security group to connect)

    ### Types of Load Balancers
      - best to use new generations
      - can setup internal or external ELBs
      #### Application Load Balancer (v2 new gen 2016 ALB)
      - Layer 7 (HTTP)
      - load balancing to multiple HTTP apps accross machines
      - load balancig to mupliple apps on the same machine (containers)
      - support for HTTP/2 and WebSocket
      - Supports redirects (HTTP to HTTPs for exmaple)
      - load balancer gets a fixed hostname (URL)
      - app servers dont see the IP of the client directly which means the instances through the balancer can see the client who sent the load directly. They have to look at extra headers in the request
        - the true IP of the client is inserted in the header called X-Forward-For
        - we can also get the port with the header > (X-Forward-port) and protocol used with the header > (X-forwarded-proto)
      - Routing tables to different target groups
        - routing based on path in URL (you can redirect these paths to different target groups)
        - routing based on hostname in URL
        - routing based on Query String, Headers
        - A great fit for micro services and container based apps (Docker and Amazon ECS)
        - has port mapping feature to redirect to a dynamic port in ECS
        - in comparison we would need multiple classic load balancers per app
      **What are target groups?**
        TYPES
        - EC2 instances (managed by Auto Scaling Group) - HTTP
        - ECs tasks (managed by ECS itself) - HTTP
        - lambda functions - HTTP request is translated ina JSOM format
        - IP addresses - must be private IPs
        ********
        - ALB can route to multiple target groups
        - health checks are at the target group level
  
       
      #### Network Load Balancer (v2 new gen 2017 NLB)
      #### Gateway Load Balancer (2020 GWLB)
