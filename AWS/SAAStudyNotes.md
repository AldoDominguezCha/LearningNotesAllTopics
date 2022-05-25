# AWS: Certified Solutions Architect Associate

<div style="page-break-after: always;"></div>

## IAM - Identity and Access Management

The Identity and Access Management is a global service, not a region-scoped one. The original root account should not be used or sahred, that's a bad security practice since it has all permissions enabled, and we need to work with the **least privilege principle: don't give more permissions to a user than the ones they strictly need for their role**.

The IAM permission policies are represented with JSON files, they define a set of permissions for making requests to AWS services, and can be used by IAM Users, User Groups, and IAM Roles, permission policies have the following properties:

1. **Version**: Specifies the language syntax rules that are to be used to process the policy JSON. IAM supports two values for this field, both are dates:

    * "2012-10-17": This is the current version of the policy language, supports features such as *policy variables*, something the older version of the policy language does not.

    * "2008-10-17": This is the older version of the policy language, it doesn't support all modern features.

2. **Id**: Optional, an identifier for the policy.

3. **Statement**: An array of one or more statement objects. The statement object has the following properties:
    
    * **Sid**: Optional. an identifier for the statement object.

    * **Effect**: *Allow* or *Deny*, whether the statement allows or denies access.

    * **Principal**:

    * **Action**: The array of actions the policy allows or denies.  

    * **Resource**: 

    * **Condition**: 

**IAM Roles**: Some AWS services will need to perform actions on our behalf, to do so, we'll asign permissions to the AWS services with IAM roles.

Some summary concepts:

- If we attach a permissions policy to a group, all the members of that group will inherit said policy by belonging to the group. An IAM user can inherit multiple permission policies from different groups they belong to.

- We can have an IAM user that doesn't belong to any group, and we can attach a permissions policy directly to that user.


1. **AWS CLI**: The AWS CLI is a tool that allows us to interact with AWS services using commands in the OS command line terminal, with the AWS CLI we have access to the public APIs of AWS services. It is open source.

2. **AWS SDK**: The AWS software development kit is a set of language-specific APIs (set of libraries) to access and manage AWS services programatically, unlike the AWS CLI, it is not something we use from the terminal, but we embed it in our application. It supports many programming languages.

Access keys are required to interact with the AWS services using the AWS CLI or any code integration with the AWS SDK


AWS CLI - List of useful commands:

- **aws configure**: Create a new configuration, we'll be prompted to input our AWS Acess Key ID and its corresponding AWS Secret Access Key, as well as the default region name.

<div style="page-break-after: always;"></div>

## Amazon EC2 - Elastic Compute Cloud

Infrastructure as a service. It consists mainly on:

- Renting virtual machines (EC2)

- Storing data on virtual drives (EBS)

- Distributing load across machines (ELB)

- Scaling the services using an auto-scaling group (ASG)

**EC2 User data script**: It allows us to bootstrap our EC2 instances, basically meaning that we can specify the launching commands when a machine starts. The script is only run once at the EC2 instance first start.

**Security Groups**: Security groups are the fundamental of network security in AWS, they control how traffic is allowed into or out of EC2 instances. Security groups only contain *allow* rules, they can reference by IP or by security group.

Security groups act like a "firewall" on EC2 instances. They regulate **access to ports, authorized IP ranges, control of inbound network and control of outbound network**.

About security groups:

- They can be attached to multiple EC2 instances, and also an instance can receive multiple security groups. It is a many to many relationship.

- Security groups are locked down to a specific region/VPC combination.

- The secutiry groups live "outside" the EC2 instances. If traffic is blocked by the security group, the EC2 instance won't see it.

- It's a good idea to dedicate an entire security group for SSH access.

- If your application is not accesible (timeout), then it is due to the security group denying the traffic.

- By default in a security group all inbound traffic is blocked, and all outbound traffic is authorized.

**SSH**: Secure Shell, is a protocol to remotely manage and modify a remote server through an authentication mechanism, all using the command line.

**Never enter your IAM access key into an EC2 instance, everyone with access to the instance will now have access to your IAM access key using the *EC2 Instance Connect* tool for example**

**Load balancing**: Load balancers are servers that forward traffic to multiple servers downstream, they spread the load across said multiple downstream instances. They expose a single point of access (DNS) to your application. *AWS Elastic Load Balancer* is a managed load balancer, AWS guarantees that it will be working. It is more costly than setting up your own load balancer, but it's really easy to implement and it is integrated with many AWS offerings/services.

An SSL certificate allows traffic between your clients and your load balancer to be encrypted in transit (in-flight encryption)






