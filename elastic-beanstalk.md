# Elastic Beanstalk
  - a developer centric view of deploying an app on AWS
  - it uses all the components weve seen before
  - managed service
    - automatically handles capacity provisioning, load balacning, caling, app health monitoring, instance configuration
    - just the app code is the responsibility of the developer
  - we still have full control over the configuration
  - beanstalk is free but you pay for the underluing instances
    ## Components
      - application : collection of Elastic Beanstalk compoents (environments, versions, configurations
      - application version : an iteration of your app code (version 1, 2 3, etc.)
      - environment:
        - collection of AWS resources running an app version (only one app version at a time)
        - tiers: web server encironment tier and worker encironment tier
          - the worker environment uses SQS queue messages instad of an ELB to send messages to each AZ and those instances in the AZ will receive those messages.
        - You can create miltiple encironments (dev, test, pro)
      - create an app > upload version > launch environment > manage environment (Or go back to uploading an update version and deloying the new version)
    ## Deployement Modes for Elastic Beanstalk
      - Single Instance (best for dev)
      - High Aailability with Load Balancer ( best for Prod)
## Instantiating Apps Quickly
 ### EC2 Instances
   - Use a Golden AMI : install apps, OS dependencies beforehand and launch your EC2 instances from the Golden AMI
   - Bootstrap using User Data : for dynamic configuration, use User Data scripts
   - Hyrib : mix of both

 ### RDS Databases
   - restore from a snapshot. the database will have schemas and data ready

 ### EBS Volumes
   - Restore from a snapshot. The disk will already be formatted and have data
