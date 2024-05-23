---
layout: posts
title: 'Day-2 Quiz-2'
section: Section-1
lesson: 3
---

## Question 1:

Which service can be used to automate image management processes?

- A) AMI

  - Incorrect: AMI (Amazon Machine Image) is used to create instances, but it does not automate image management processes.

- B) EC2 Image Builder

  - Correct: EC2 Image Builder is a service that simplifies the creation, maintenance, validation, and sharing of images.

- C) EBS Snapshots

  - Incorrect: EBS Snapshots are used for backing up EBS Volumes, not for automating image management.

- D) IAM
  - Incorrect: IAM (Identity and Access Management) is used for managing access to AWS services, not for image management.
  <!-- pagebreak -->

## Question 2:

Which EC2 Storage would you use to create a shared network file system for your EC2 Instances?

- A) EBS Volume

  - Incorrect: EBS Volumes are block storage devices that can be attached to EC2 instances but do not provide a shared file system.

- B) EC2 Instance Store

  - Incorrect: EC2 Instance Store provides temporary block-level storage for an instance, not a shared network file system.

- C) EBS Snapshots

  - Incorrect: EBS Snapshots are backups of EBS Volumes, not a file system.

- D) EFS
  - Correct: EFS (Elastic File System) is a fully managed file storage service that can be mounted on multiple EC2 instances, providing a shared file system.
  <!-- pagebreak -->

## Question 3:

Which statement is correct regarding EC2 Instance Store?

- A) It is not good to use as a disk to cache content

  - Incorrect: EC2 Instance Store can be used for temporary storage or caching content.

- B) It has a better I/O performance, but the data is lost if the EC2 Instance is terminated

  - Correct: EC2 Instance Store provides better I/O performance, but the data does not persist if the instance is stopped or terminated.

- C) Your data is always safe with EC2 Instance Store
  - Incorrect: Data stored in EC2 Instance Store is ephemeral and will be lost if the instance is terminated.
  <!-- pagebreak -->

## Question 4:

Which of the following is a fully managed native Microsoft Windows file system?

- A) EFS

  - Incorrect: EFS is a fully managed file system for Linux-based workloads.

- B) FSx

  - Correct: FSx for Windows File Server provides a fully managed, highly reliable, and scalable Windows native shared file storage.

- C) EBS
  - Incorrect: EBS is a block storage service and does not provide a fully managed Windows file system.
  <!-- pagebreak -->

## Question 5:

An EBS Volume is a network drive you can attach to your instances while they run, so your instances' data persist even after their termination.

- A) True

  - Correct: An EBS Volume provides persistent storage that can be attached to EC2 instances. The data remains even after the instance is terminated.

- B) False
  - Incorrect: This statement is true. EBS Volumes provide persistent storage.
  <!-- pagebreak -->

## Question 6:

EBS Volumes CANNOT be attached to multiple EC2 instances at a time.

- A) True

  - Correct: EBS Volumes are designed to be attached to a single instance at a time. However, Amazon EBS Multi-Attach enables you to attach a single Provisioned IOPS SSD (io1) volume to up to 16 Nitro-based instances that are in the same Availability Zone.

- B) False
  - Incorrect: By default, EBS Volumes cannot be attached to multiple instances, except in the case of EBS Multi-Attach.
  <!-- pagebreak -->

## Question 7:

What are AMIs NOT used for?

- A) Add your own software license

  - Incorrect: You can include your own software license in an AMI.

- B) Add your own configuration

  - Incorrect: You can customize an AMI with your own configuration.

- C) Add your own operating-system

  - Incorrect: You can create an AMI with your preferred operating system.

- D) Add your own IP addresses
  - Correct: AMIs do not include IP addresses. IP addresses are assigned to instances when they are launched.
  <!-- pagebreak -->

## Question 8:

What is an EBS Snapshot?

- A) The operating-system on an EC2 Instance

  - Incorrect: An EBS Snapshot is not related to the operating system.

- B) A backup of your EBS Volume at a point in time

  - Correct: An EBS Snapshot is a point-in-time backup of an EBS Volume.

- C) The amount of CPU and RAM of an EC2 Instance
  - Incorrect: EBS Snapshots are related to storage, not compute resources like CPU and RAM.
  <!-- pagebreak -->

## Question 9:

Where can you find a third party's AMI so you can use it to launch your EC2 Instance?

- A) Public AMIs

  - Incorrect: Public AMIs are shared by AWS users and AWS itself, but they might not necessarily be third-party AMIs.

- B) My own AMIs

  - Incorrect: This option only includes AMIs that you have created or shared with your account.

- C) AWS Marketplace AMIs
  - Correct: AWS Marketplace offers third-party AMIs that you can use to launch your EC2 instances.
  <!-- pagebreak -->

## Question 10:

What is an EBS Volume tied to?

- A) A region

  - Incorrect: EBS Volumes are tied to a specific Availability Zone within a region.

- B) A data center

  - Incorrect: EBS Volumes are tied to Availability Zones, not to individual data centers.

- C) An edge location

  - Incorrect: EBS Volumes are not associated with edge locations, which are used for content delivery.

- D) An availability zone
  - Correct: EBS Volumes are tied to a specific Availability Zone, and they can only be attached to instances within the same zone.
