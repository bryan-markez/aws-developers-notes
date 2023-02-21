# EC2 Fundamentals

## Personal notes
[02-02-23] need to brush up my knowledge on EC2 since I mainly focus on serverless in AWS

## EC2 Configuration options
- OS
- CPU (compute power and cores)
- RAM
- Storage Spaces
  - Network-Attached (EBS & EFS)
- Network card
- Security Groups (firewall)
- bootscript script (EC2 User data script)
  - configure the instance at launch, only run on first start
  - script will run as root user

## EC2 Instance Types
Take for example `m5.2xlarge`:

- m: instance class
- 5: generation
- 2xlarge: size of of this particular instance

Types to know:
- General Purpose
  - good balance of compute, memory, and networking
  - Good to start with until you can identify which performance you need to specialize in more
- Compute Optimized
  - good for tasks that require high performance processors
    - batch processing workloads, media transcoding, HPC, dedicated gaming servers
- Memory Optimized
  - good for optimizing the performance of workloads that requires to process a large amount of data
    - relational/non-relational databases
    - web cache stores
    - BI
    - applications that focus on real-time processing on big unstructured data
- Storage Optimized
  - good tasks that require high, sequential read and write access to large data sets on local storage
    - OLTP systems
    - Relation and NoSQL databases
    - Cache
    - Data warehousing applications

## Security Groups
Firewalls of EC2, fundamental of network security in AWS

Control how traffic is allowed into or out of EC2 instances, they regulate:
- access to parts
- authorized IP ranges
- control of Inbound/Outbound networks

By default:
- all inbound traffic is blocked
- all outbound traffic is allowed

When referencing other security groups in a security group:
- you can reference "SG B" in "SG A". This means that any instances that have "SG B" attached can communicate with instances that have "SG A" attached.

*Some gotchas*:
- Security groups are locked down to that region/vpc. For each region/vpc you will need to create another security group, you can't reuse the security group from VPCA to an EC2 instance living in VPCB
- Time out errors = security group issue
- Connection refused error = application error / instance is not online

## EC2 Purchasing Options
- On-Demand
  - pay by second
  - good for short workload
- Reserved
  - 1 or 3 years
  - good for long workloads (databases)
  - optional for "Convertible Reserved Instances" so that you can still modify the instance type if you need to adjust performance / scale
  - can pay some amount upfront for a cheaper price
- Savings Plan
  - 1 or 3 year
  - commit to amount of usage (ex $10/hour for 1 or 3 years)
  - locked instance family and region
- Spot Instances
  - cheapest option
  - good short work loads, or workloads that are resilient to failure
  - NOT RECOMMENDED FOR ANY TASKS THAT YOU DON'T WANT TO LOSE DATA
- Dedicated Host
  - book an entire physical server
- Dedicated Instances
  - no other customer will share your hardware
- Capacity Reservations
  - reserve capacity in a specfic AZ for any duration

## EC2 Instance Store
Physical hardware disk attached to EC2 (not a network drive)
- typically provides better IOPs
- instance store is ephemeral
- good for buffer/cache/temporary content
- Risk of data loss if hardware fails

Backup and replication is on the user, not AWS

## EBS
Elastic Block store is a network drive you can attach your instances while they run
- typically can only be mounted to one instance at a time, but there are options for "multi-attach" feature
- bound/locked to a specific AZ
- it could be detached from one instance to be attached to another
- provisioned capacity
  - can increase capacity over time

For snapshots:
- can make a backup at a point in time
- can copy across AZ or Region
- Theres an archive tier to make storing EBS cheaper, however it will take longer to restore the archive (24-72 hours)
- you can set up rules to retain deleted snapshots in case of accidental deletions

Volume types:
- General SSD
  - gp2/gp3
  - balances price and performance for a wide variety of workloads
- High Performance SSD
  - io1/io2
  - **provisioned IOPs**, sustained performance
  - for mission-critical low-latency / high-throughput
  - can support multi-attach
    - note, the application is responible for managing concurrent write operations
    - up to 16 EC2 instances at a time
    - must use a file system that's cluster-aware
- Low Cost HDD
  - st1
  - cheap, slower than the SSD
- Lowest Cost HDD
  - c1
  - lowest cost, typically used for workloads that don't require to very fast latency

Only SSD volumes can be used as boot volumes

Provision 

## EFS
Elastic File System is a managed NFS that can be mounted on many EC2
- allows multi-AZ
- expensive
- pay-per-use

Good for content mangement, web service, data sharing, wordpress

Requires SG setup to access EFS

Has storage tiers:
- can set up lifecycle policies so that files can be moved to a different storage tier after N days
- EFS-IA(infrequently accessed), cost to retrieve file but cheaper to store

Can make EFS either Multi-AZ (standard) or one zone (good for dev)

Uses POSIX file system

## AMI
Amazon Machine Image (customization of a EC2 instance)
- software, configuration, os, monitoring, etc...
- faster boot/configuration time because software is pre-packaged
- AMI is built for a specific region (be can be copied across regions)

To create an AMI:
1. start an ec2 instance then customize it
2. stop the instance
3. create the AMI image (it will also create an EBS snapshot)
4. now you can start launching instances from other AMIs