# Day-2 Quiz 1

## Question 1:

Which EC2 Purchasing Option can provide the biggest discount, but is not suitable for critical jobs or databases?

- A) Reserved Instances  
  Incorrect: Reserved Instances provide a discount but are suitable for steady-state usage.

- B) Convertible Instances  
  Incorrect: Convertible Instances offer flexibility to change instance types but are not the biggest discount option.

- C) Dedicated Hosts  
  Incorrect: Dedicated Hosts are for compliance and regulatory requirements, not for cost savings.

- D) Spot Instances  
  Correct: Spot Instances provide the biggest discount but can be interrupted by AWS, making them unsuitable for critical jobs or databases.

## Question 2:

Which network security tool can you use to control traffic in and out of EC2 Instances?

- A) Network Access Control List (NACL)  
  Correct: NACLs control traffic at the subnet level, acting as a firewall for controlling traffic in and out of one or more subnets.

- B) Identity and Management Access (IAM)  
  Incorrect: IAM is used for identity and access management, not network traffic control.

- C) Guard Duty  
  Incorrect: GuardDuty is a threat detection service, not a network traffic control tool.

- D) Security Groups  
  Correct: Security Groups act as virtual firewalls for your instances to control incoming and outgoing traffic.

## Question 3:

Under the Shared Responsibility Model, who is responsible for operating-system patches and updates on EC2 Instances?

- A) The customer  
  Correct: The customer is responsible for maintaining and patching the guest operating system and any application software or utilities installed by the customer on the EC2 Instances.

- B) AWS  
  Incorrect: AWS is responsible for the infrastructure and underlying hardware, not the operating system on EC2 Instances.

- C) Both AWS and the customer  
  Incorrect: Only the customer is responsible for the operating system patches and updates on EC2 Instances.

## Question 4:

How long can you reserve an EC2 Reserved Instance?

- A) 1 or 3 years  
  Correct: You can reserve an EC2 Reserved Instance for either a 1-year or 3-year term.

- B) 2 or 4 years  
  Incorrect: EC2 Reserved Instances are not available for 2 or 4-year terms.

- C) 6 months or 1 year  
  Incorrect: EC2 Reserved Instances are not available for 6-month terms.

- D) Anytime between 1 and 3 years  
  Incorrect: EC2 Reserved Instances have fixed terms of 1 or 3 years.

## Question 5:

A company would like to deploy a high-performance computing (HPC) application on EC2. Which EC2 instance type should it choose?

- A) Compute Optimized  
  Correct: Compute Optimized instances are ideal for compute-bound applications that benefit from high-performance processors.

- B) Storage Optimized  
  Incorrect: Storage Optimized instances are designed for workloads that require high, sequential read and write access to very large data sets.

- C) Memory Optimized  
  Incorrect: Memory Optimized instances are designed to deliver fast performance for workloads that process large data sets in memory.

- D) General Purpose  
  Incorrect: General Purpose instances provide a balance of compute, memory, and networking resources and can be used for a variety of diverse workloads.

## Question 6:

Which of the following is NOT an EC2 Instance Purchasing Option?

- A) Spot Instances  
  Incorrect: Spot Instances are an EC2 instance purchasing option that allows you to bid on spare EC2 capacity.

- B) Reserved Instances  
  Incorrect: Reserved Instances are an EC2 instance purchasing option that provides a discount compared to On-Demand pricing.

- C) On-demand Instances  
  Incorrect: On-demand Instances are an EC2 instance purchasing option that lets you pay for compute capacity by the hour or second.

- D) Connect Instances  
  Correct: Connect Instances are not a purchasing option for EC2. The options are On-demand, Reserved, and Spot Instances.

## Question 7:

Which EC2 Purchasing Option should you use for an application you plan on running on a server continuously for 1 year?

- A) Reserved Instances  
  Correct: Reserved Instances are ideal for applications with steady-state usage and offer significant cost savings compared to On-Demand instances.

- B) Spot Instances  
  Incorrect: Spot Instances are suitable for flexible start and end times but are not ideal for continuous usage as they can be interrupted.

- C) On-demand Instances  
  Incorrect: On-demand Instances are suitable for short-term, irregular workloads but are more expensive for continuous usage.

- D) Convertible Instances  
  Incorrect: Convertible Instances offer flexibility to change instance types but are not necessarily the best option for continuous usage without the cost savings of Reserved Instances.
