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
  - Add name and tags
  - Select an AMI (Amazon Machine Image)
  - Select instance type. You can compare them if needed
  - Create a key pari (login)
    - Create pair name
    - Key pair type : RSA
    - Select private key file format. It will be downloaded for you.
      - Windows 7 and Windows 8 use .ppk
      - Everything else use .pem
  - Network setting
    - Change security group which controls the traffic to and from the instance
  - Configure the storage you want. Click advanced to see more details
  - Advanced Details
    - User Data : pass a script (a set of commands ( bash .sh file)) that will be executed on the first launch of your EC2 instance

  **********
  - the public IP address will change when you start and stop an instance put the private IP address will not

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
