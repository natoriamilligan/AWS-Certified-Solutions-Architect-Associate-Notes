# EC2
  - one of the most popular services on AWS
    
  ## EC2 sizing and Configuration options
  - You can choose hoe you want your virtual server to be
  **What can we choose for our instances (virtual servers that we rent)**
    - Operating System (Linux, Windows or Mac OS)
    - CPU (how much compute power and cores)
    - How much RAM
    - How much storage space
      - network attached (EBS & EFS)
      - hardware (EC2 Instance Store)
    - Network card: speed of card, what kind of Public IP address do you want
    - Firewall rules (security group)
    - Bootstrap script (configure at first launch): EC2 User Data
   
  ## EC2 User Data (Bootstrapping)
    - bootstrapping: launging commands when the machine starts. The script will only run once at the instance first start.
    - EC2 user data is used to automate boot tasks:
      - installing updates
      - installing software
      - downloading common files from the internet
    - EC2 Data user runs with the root user
      
  ## Instance Types
    - 6 Types. Each Instance type has different families. (Compare at instances.vantage.sh)
      - **General Purpose** (M, T)
        - great for a diversity of workloads
        - balanced between compute memory and networking
      - **Compute Optimized** (C)
        - For compute intensive tasks that require high performance processors
          - gaming, high performance web servers
          - media transcoding
          - Machine learning
          - etc
      - **Memory Optimized** (R)
        - good for workloads that process large data sets in memory
          - high performance relational/non-relational databases
          - processing big unstructured data
          - etc
      - **Storage Optimized** (I, D, H1)
        - for storage instensive tasks that require high requential read and write access to large data sets on local storage
          - data warehousing apps
          - cache for in-memory databases
          - relations and NoSQL databases (Redis)
          - high frequency online transaction processing (OLTP) systems
    - Naming convention
      Ex. m5.2xlarge
        - m : instance class
        - 5 : generation
        - 2xlarge : size within class
  
  ## How to run an EC2 Instance on Linux
    1. Add name and tags
    2. Select an AMI (Amazon Machine Image)
    3. Select instance type. You can compare them if needed
    4. Create a key pari (login)
      - Create pair name
      - Key pair type : RSA
      - Select private key file format. It will be downloaded for you.
        - Windows 7 and Windows 8 use .ppk
        - Everything else use .pem
    5. Network setting
      - Change security group which controls the traffic to and from the instance
    6. Configure the storage you want. Click advanced to see more details
    7. Advanced Details
      - User Data : pass a script (a set of commands ( bash .sh file)) that will be executed on the first launch of your EC2 instance
  
    **********
    - the public IP address will change when you start and stop an instance put the private IP address will not
  
  ## How to run the script in Linux
    1. Make sure your .pem file is downloaded
    2. Add the .pem file to the directory of your choice
    3. Run `ssh -i pemFileName ec2-user@publicIPaddressofInstance`
    4. If need be you might need to change modifications of the pemfile `chmod 0400 nameofpemfile`
    5. You can ping google.com then ctrl c to check your connection
    6. type  `exit` or `ctrl g` to close ec2 instance
  
    - You can use EC2 Connect to connect to the terminal on the brower. Click the connect tab above your instance
  
  ## Security Groups
    - fundamental to network security in AWS
    - controls how traffic is allowed into the instances
    - contain allow rules
    - rules can reference by IP or by security group
    - security groups act like a firewall around the EC2 instance
    - security groups regulate
      - access to ports
      - authorized IP ranges - IPv4 and IPv6
      - control inbound and outbound network
    - can be attached to multiple instances
    - instances can have multiple security groups
    - locked down to region/VPC combination
    - lives outside the EC2 - if traffic is blocked EC2 wont see it
    - good to maintain one separate security groups for SSH access
    - if your app is not accessible (time out) its a security group issue
    - if your app givs a "connection refused erros, its an app error or its not launched
    - all inbound traffic is blocked by default
    - all outbound traffic is authorized by default
    - if different instances have the correct authorized security group attached to them, they can communicate with other instances.
      ### Ports
        - 22 : SSH (Secure Shell) - log into a Linux instance
        - 21 : FTP (file transfer protocol) - upload files into file share
        - 22 : SFTP (Secure File Transfer Protocol) - upload files using SSH
        - 80 : HTTP - access unsecured websites
        - 443 : HTTPS - access secured websites
        - 3389 : RDP (Remote Desktop Protocol) - log into a Windows instance
  
  ## IAM Roles For EC2
    **Never added your access key credentials to an EC2 instance bc other users who log in will be able to see your keys. So use IAM Roles instead**
    1. Go to Actions > security > modify IAM Role in your instances page

  ## EC2 Purchasing Options
  1. On-Demand : short workload, predictable pricing, pay by second
     - Pay for what you use:
       - Linux or Windows - billing per second, after first minute
       - all other OSs, billing per hour
     - Highest cost but no upfron payment
     - no long term commitment
     - for short term/uninterrupted workloads, where you cant predict how the app will behave
  3. Reserved (1 or 3 years)
     - Reserved instances : long workloads
       - 72% (subject to change) discount compared to On-Demand
       - reserved a specfic instance attributes (InstanceType, Region, Tenancy , OS)
       - Reservation Period - 1 year(+discount) or 3years(+++discount)
       - Payment Options - No upfront(+), partial upfront(++), or all upfront(+++)
       - Reserved Instance's Scope = Regional or Zonal (reserve capacity in an AZ)
       - Recommended For steady state usage apps (databases)
       - You can buy and sell in the Reserved Instance Marketplace
     - Convertiable Reserved Instances : long workloads with flexible instances
       - Can change the EC2 instance type, instance family, OS, scope and tenancy
       - Up to 66% discount
  5. Savings Plans (1 or 3 years) : commitment to an amount of usage, long workload
     - same as reserved pretty much but more flexible
     - Discount based on long term usage
     - comit to a certain type of usage ($10/hour for 1 or 3 years)
     - any usage beyond the savings plan is billed at on-demand price
     - Locked to a specific instance family & AWS region
     - Flexible across instance size and OS, and tenacy (Host, Dedicated, Default)
  7. Spot Instances : short workloads, cheap and inreliable
     - Most agreesive discounts (up to 90%)
     - you can lose instances at any point in time if max price is less than the current spot price (you define a max price you are willing to pay for your spot instances, if they go over, you lose it)
     - The MOST cost-efficient instances in AWSD
     - Useful for workloads that are resiliant to failure
       - batch jobs
       - data analysis
       - image processing
       - distributed workloads
       - workloads with flexible start and end times
      - **Not suitable for databases or critial jobs**
      - Sport Blocks: no longer in usage however they block a spot instances during a specific time frame
      - Spot price varies based on Region
      - You get a 2 minute grace period if price goes oever, you can either stop or terminate. If the spot price goes down, you can just stop and restart with the price goes back down. OTherwise you might just choose to terminate and restart a new instance on another spot.
      - Spot Requests
        - requests that define the amount of instances you want, costs and time frame. The request can be persistant or a one time request.
        - Also if instances are closed because of price, the spot request will open another for you
        - If it is persistent and you terminate an instance, your spot request is still open and will create instances to make them match the spot request.
        - It is best to stop your spot request before you terminate instances.
      - Spot Fleets = set of spot instances + (optional) On Demand Instances
        - the fleet will try to meet the target capacity with the price contraints you give it
          - you can define the isntance types, OS, and AZs
          - You can have multiple launch pools so the fleet can choose the best
          - the spot fleet will stop launching instances when it reaches capacity or the max cost
          - Strategies to allocate Spot Instances for the fleet to work with:
            - lowest price : from the pool with the lowest price (cost optimized, short workloads)
            - diversified : distributed accross all pools (great for availibilty and long workloads)
            - capacity optimzed : pool with the optimal capacity for the number of instances
            - priceCapacityOptimzed(recommended) : pools with the highest capacity, then select the pool with the lowest price (best choice for most workloads)
          - spot fleets allow us to automatically request Spot nstances with the lowest price
  9. Dedicated Hosts : book an entire server, control instance placement ( you have control over the actual server while dedicated instances do not) (gives you visibility into the lower level hardware)
      - A physical server with EC2 instance capacity fully dedicated to your use
      - useful if you have compliance requirements and use your existig server-bound software licenses (per socket, per-core,per-VM software licenses)
      - Purchasing Options
        1. On demand - pay per second for active Dedicated Host
        2. Reserved (1 or 3 years) (No upfront, partial upfront, All upfront)
      - Most expensive
      - Choose this if you have software with complicate licensing models (BYOL) or for companies that have strong regulatory or compliance needs
  11. Dedicated Instances : no other customers will share your hardware
      - instances that run on hardware thats dedicated to you
      - you may share hardware with other instancs in the same account
      - no control over instance placement (can move hardware after stop / start)
  13. Capacity Reservations : reserve capacity ina specfic AZ for any duration
      - reserve on-demand instanvces in a specific AX for any duration
      - you always have acces to EC2 capacity when you need it
      - no time commitment, no billing dicounts
      - combine with  Regional Reserved Instances and SAvings Plans to benefit from discounts
      - Your charged at ondemand-rate whether you run instances or not
      - Best for short term, uniterrupted workloads that needs to be in a specific AZ
  ### How to choose which purchaing option is for me?
  - On demand : You walk into a car rental place and say, “I need a car for a few hours or days.” You pay only for what you use, and you can return it anytime.
  - Reserved : You sign a 1-year or 3-year lease on a car. Because you commit for longer, you get a much lower monthly rate.
  - Savings Plan : Instead of leasing one specific car, you commit to spend $X per month on cars over 1–3 years. You can drive different models (SUVs, sedans, etc.), and the company gives you a discounted rate across your usage.
  - Spot instances : The car rental place calls you and says, “We’ve got a bunch of unused cars today — you can rent them super cheap, but if we need them back, you have to return them right away.”
  - Dedicated Hosts : You rent the entire garage with one or more cars exclusively for yourself. You know exactly which cars are there, you choose which to drive, and no one else ever touches them.
  - Dedicated Instances : You always get a car that no one else has used or will use, but you don’t get to pick which garage it’s in, or know if all your cars are in the same garage.
  - Capacity : You call the rental agency and say, “I’ll need an SUV on Friday at 3 PM — hold it for me.” They guarantee that the car will be waiting when you need it. Reserve in a spcific AZ. you are guarateed no matter what.

## Private and Public IP
  - IPv4 is still the ost commonly used
    - 4 numbers separated by periods (each number is between 0-255)
    - Allows for 3.7 billons different addresses in the public space
  - IPv6 is comstly used for the IoT devices because it is newer and soves problems for them.
    - has a more complicated IP number format
  ********
  - Your EC2 machine comes with:
    - a private Ip for the internal AWS network
    - a public IP for the web
  - During SSH into our EC2 machine
    - we can use a private IP because we are not on the same network (need a VPN)
    - We can only use the public IP
  ### Public IPs
  - machine can be identified on the internet
  - must be unique accross the whole web
  - can be geo-lacated easily

  ### Private IPs
  - machines can only be identified on a private network
  - IP must be unique accross the private network but 2 different private networks can have the same IPs
  - machines connect to the web used an internet gateway (a proxy)
  - only a specified range of IPs can be used as a private IP

  ### Elastic IPs
  - when you start an EC2 instance, it can change its public IP
  - can only have 5 elastic Ips in your account
  - avoid using them. Instead used a random public IP and register a DNA name to it
  - if you need to have a fixed piblic IP for your instance, you need an Elastic IP
  - its public as long as your dont delete it
  - You can attach one instance to it at a time
  - with an elastic IP, you can mask the failure of an instance or software by remapping the address to another instance in your account
  #### Create an Elastic IP address and associate it to an EC2 instance
  - Create an Elastic IP address
  - Then from actions > associate IP address > click instance > search for instance to use > Private IP address selection (choose the Elastic IP address)
  - Disassociate the IP address, then release the IP address so you do not get charged

## EC2 Placement Groups
  - Control over the EC2 instance placement strategy
  - Create a placement group and when you create an instance, in advanced details, you can add that instance to a placement group
  - Strategies:
    - Cluster : clusters instances into a low latency group in a single AZ
      - pros : great networking bc you are in same AZ (10 GBPS bandwith)
      - cons : if the AZ fials, all instances fail at the same time
      - use-case : big data jobs that will be completed fast
      - apps that need extremely low latency and high network throughput
    - Spread : spreads instances across underlying hardware (max 7 instances per group per AZ) - critical apps
      - for minimizing failure risk
      - all instances are located on different hardware
      - pros : spread accross AZs, reduced risk of failure
      - cons : limited to 7 instances per AZ per placement group
      - use case : app that needs to maxime high availability and critical apps where each instance must be isolated from failure
    - Partition : spread instances across many different partitions (which rely on different sets of racks) within an AZ. Scales to 100s of EC2 instances per group
      - instances spread across partitions in many AZs in the same region
      - you can use many instances in each partitions
      - each partition is a rack so each partition is safe from a rack failure in the hardware
      - up to 7 partitions per AZ
      - use cases : partition aware apps, big data
     
## EC2 Network Interface (ENI)
  - Represents a virtual network card
  - An ENI can have the following:
    - Primary private IPv4, one or more secondary IPv4
    - One Elastic IP per private IPv4
    - One Public IP
    - One or more security groups
    - A MAC address
  - You can create ENIs independently and attach them or move them on EC2 instances for failover
  - Bound to a specific AZ
  - When you create an independent ENI, it gets a private IP address

## EC2 Hibernate
  - Option other than stopping or terminating an instance
  - RAM state is preserved in an encrypted and written in the root EBS volume where the EBS volume is what is encrypted. The EC2 Instance Volume Type has to be an EBS volume.
  - Used for long running processes, saving the RAM state, or if you have services that take a lot of time to initialize
  - Instance RAM size should be less than 150 GB
  - works for many OSs, instance families, AMIs, ondemand, reserved, and spot instances.
  - Not supported for bare metal instances
  - An instance cant be hibernated for no more than 60 days
  #### How to set up Hibernate on an instance?
   1. Create your instance
   2. Go to advanced setting and check Hibernate Behavior > enable
   3. Then go back up to Storage Options > Advanced > encrypt the root volume > select the default KMS key to encrypt
   4. Make sure the size of the volume can hold the tier you are using. You can check the tier size if you scroll up to Instance Type
   5. Once your instance is up and running you can hibernate the instance if needed

## EC2 Storage Options
  ### EBS Volumes (Elastic Block Store)
  - a network drive you can attach to instances while they run
  - allows you instances to persist data, even after their termination. You can make another instance and add the volume and get back the data
  - They can only be mounted to one instance at a time (sometimes have a multi attach feature?)
  - bound to a specific AZ (or you have to snapshot it first)
  - They are like a network USB stick
  - it uses the network to communicate to the instance so their might be latency
  - it can detach from an instance quickly
  - you have to provision the capacity in advance.
    - you will get billed for the capacity
    - you can change over time
  - they can be create independently to be attached on demand
  - mulitple EBS volumes can be attached to a single instance
  - The root volume is automatically checked to delete on termination but subsiquent ones added are not checked for that. However you can change what you want to happen for both the root volume and other volumes
  
        
    

  
