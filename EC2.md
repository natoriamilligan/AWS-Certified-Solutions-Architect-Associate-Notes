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
