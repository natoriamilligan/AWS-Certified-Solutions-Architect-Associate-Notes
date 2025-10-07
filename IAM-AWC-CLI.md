# IAM & AWS CLI (Global Service)

## IAM
  - Identity and Access Management
  - to be able to sign in using users, you need to create an alias from the root user
  ### Users & Groups
  - Never use the root account or share it instead users
  - Users are people within your organization and can be grouped together
  - groups only contain users not other groups
  - Its not good practice, but users dont have to belong to a groups
  - Users can also belong to multiple groups
    ### Permissions
    - We use groups to assign groups permissions
    - users/groups are assigned JSON docs called a policy which allows which services they can use
    - policies help define permissions for users
    - inline policies are only attached to a user (useful when a user doesnt belong to a group)
      #### Policy Structure
      - `Version` : policy language version "2012-10-17"
      - `Id` : identifier for the policy (optional)
      - `Statement` : one or more individual statements
        - `Sid` : identifier for the statement
        - `Effect` : whether the statement allows or denies access (Allow or Deny)
        - `Principle` : account/user/role to which the policy is applied to
        - `Action` : list of actions this policy allows or denies
        - `Resource` : list of resources to which the actions applied
        - `Condition` : conditions for when this policy is in effect (optional)
    ### Passwords and MFA
      - You can customize a password policy for your users
      -  MFA is a must and is recommended V
      -  Virtual MFA device (Google Authenticator or Authy)
         - Universal 2nf Factor (YubiKey)
         - Hardware Key Fob Device
         - Hardware Key Fob Device for AWS GovCloud
       
    ### How to access AWS?
      - AWS Management Console : protected by MFA and password
      - AWS Command Line Interface : protected by access keys (CLI)
      - AWS Software Development Kit : for code; protected by access keys (SDK)
        - language specific APIs
        - enables you to access AWS programmatically

    ### IAM Roles
    -  permissions given to AWS services
    -  **POLICY**
        - "Action": ["sts:AssumeRole"]
        - "Service": ["ec2.amazonaws.com"]
        - This means that this role can be assumed by the EC2 service

    ### IAM Security Tools
    - IAM Credentials Report (account-level)
      - lists account users and the status of their credentials
      - great to look at if you are looking at users account security and if they are changing their passwords or have MFA
      - CSV file
      - In IAM left tab
    - IAM Last Accessed (user-level)
      - shows service permissions granted to users and when those services were last accessed
      - use this info to revise permissions
      - find tab in the individual users profile
  ## AWS CLI
  - Allows you to access AWS through the command line
    ### Access Keys
      - Used to access the CLI
      - After you create an access key, that is the last time you will see that page. You have to store your access key somewhere or dowmload it
      **How to get an access key?**
      - Go to user
      - Security creditials
      - Create access keys
   
    ### How to use the CLI?
      - First install aws cli via the terminal
      - `aws configure`
      - enter aws access key and aws secret access key
      - add region name
      - default output press enter
    
    
         
         
  
  
