# IAM & AWS CLI (Global Service)

## IAM
  - Identity and Access Management
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
        -  MFA is a must and is recommended
  
