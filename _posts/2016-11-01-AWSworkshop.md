---
layout: post
title: Amazon AWS workshop at the University of Sydney
comments: true
tags:
- seminarsattended
---

On October 24th I attented a talk by Adrian White about using Amazon AWS for research. Below are my notes, not extensively annotated, in the hope that they'll be useful for someone. If something is not clear, please ask in the comments, and I'll try to answer to the best of my knowledge


# 161024\_AWS workshop

- SciTeam ~10 ppl

## Overview of AWS services

- If you need to make your data available you have availability zones. For an ultra-stable website we can make our app accessible in multiple AZs. 

- Networking - pair with AARNet in Australia

- spark to run GATK

- ML module - developed for ecommerce based on how people visit websites
- supervised learning only, basic linear regression


## Essential services
### Elastic compute (EC2) 
- elastic load balance capability interesting for when you're firing up a shiny analytics environment

### Networking Services
#### Amazon VPC (virtual private cloud)
- own network isolated, within a region, you control addressing, DNS servers, whether it connects to the internet

#### AWS DirectConnect
- if you need to transfer data faster

#### Amazon Route 53
- Domain Name System (DNS) web service.
### Storage
#### Amazon S3
- object storage service (put/get/delete) not a file system (so can't do byte range retrievals and other subsets of the files)
- durability with a 10^6 objects you have 1:10k chance of losing it
- File size - up to 5 Tb.

#### Amazon EBS
- up to 16 Tb
- disk that fires up and is mounted to a server on EC2
- can still use magnetic disk to cut cost (default is SSD)
#### Glacier
- file size up to 50 Tb
- cold storage
- can take 3-5 hours to retrieve object
- also 11 9s durability
#### AWS Storage Gateway
- storage on premise
### Databases
### Amazon RDS
- Standard relational database. Maintained/run by amazon so you can just use the data
#### Amazon DynamoDB
- Managed NoSQL database service
- today need 100 reads/sec, tomorrow 1000k

#### Amazon ElastiCache
- reddis 
- some there open source libraries


### Big Data Services
- Amazon EMR (Elastic Map Reduce)
- AWS Data Pipeline Hosted Hadoop framework
	- Move data among AWS services and on premises data sources
- Amazon Redshift
	- Petabyte-scale data warehouse service
	- OLAP style DB environment (massively parallel processing database)

### Monitoring services
- Amazon CloudWatch (free)
	- Monitor resources
	- you can make your own monitors such as how many times a function is run and send you notifications or make graphs
- AWS IAM (Identity & Access Mgmt)
	- Manage users, groups & permissions
- AWS OpsWorks
	- Dev-Ops framework for application lifecycle management
- AWS CloudFormation
	- Templates to deploy & manage
	- This is useful for us. You build a cluster based on templates = users ,roles, storage, databases, etc 
	- infrastructure as a configuration file
- AWS Elastic Beanstalk
	- Automate resource management
	- can host apps with python or java or docker containers (find out more)

### Accessing your resources

- Everything you can do through the Console, you can do through the CLI or an SDK
- SDK’s for most programming languages
	- Andriod, IOS, Java, .Net, Node.js, PHP, Python, Ruby, Go

#### Commercial models

- on-demand
	- On-Demand Pay for compute capacity by the hour with no long-term commitments For spiky workloads, or to define needs
- spot
	- Bid for unused capacity, charged at a Spot Price which fluctuates based on supply and demand For time-insensitive or transient workloads
	- 60-70% savings on on demand price when it's on Spot
	- if price goes above that cost, it gets shut down in 2 minutes
	- RNA-seq on spot to have disappearing servers on 250 k samples
- Reserved
	- Make a low, one-time payment and receive a significant discount on the hourly charge. For committed utilization
- Dedicated
 	- you are the *only* user on a particular physical piece of hardware
 	- health care data compliancy (in the US)
 		
Amazon won't kill jobs for you itself if your bill goes higher than X. You can configure this yourself, specifying what should happen to storage. 


### Security

### Popular HPC workloads on AWS
- Genome processing
- Modeling and Simulation
- Government and Educational Research
- Monte Carlo Simulations
- Transcoding and Encoding (video)
- Computational Chemistry

### Educate
- aws.amazon.com/training/- self-paced-labs
- aws.amazon.com/training
- aws.amazon.com/certification

Can also be used in class:


- AWS blog - best place to get updates on the main stuff that changes
- Deeper into a topic - product page + documentation



# Moving data to AWS
### Storing data in S3 and EC2
Amazon S3 is object based storage

- Data is an object (treat each file as a single object)
- Consists of data (globally unique identifier) and metadata
- Very simple operations: (not POSIX!)
- PUT, GET, DELETE, LIST
- Cannot do an lseek, partial read or write, over-write existing data
- Versioning is allowed (only differences are stored)
- You control where data is located (Amazon never moves or copies it)
- Flat hierarchy (no concept of directories)

### Amazon S3 is
-  Extremely Durable (99.999999999%)
-  Extremely Available (99.99%)
-  Virtually infinite (scalable)
-  Pay only for what you use
-  You control who has access to your data (and the location of the data)
-  Data Encryption
	- not encrypted by default
	- can tick a box to get amazon to manage encryption
	- can be completely client side encryption only
-  Event notifications
-  Lifecycle Management
- Amazon S3 is more read oriented than write oriented
-  However, write performance is still very good

QIMR: Moving data to Europe via snowball - a data store device 

- Optimising data transfer to S3
- Examples


### Preliminaries

#### Before worrying about upload performance:
1. Know file size distribution and the amount of data
	- Average file size? Largest? Smallest? Standard Deviation?
2. Know how you are connected to Internet and AWS
	- Perform preliminary tests to S3 to understand network performance
3. Know how you will use data in an Amazon S3 bucket
	- Will it be shared?
	- How is it organized? How do applications access the data?
	- Read vs. Write?
	- Being able to answer these questions will make things much, much easier (with faster uploads)


### Non-Network Keys to the fastest upload
- The following are general principles for the quickest data
upload from outside of AWS
- Multi-part upload
	- aws s3 create-multipart-upload
	- **aws s3 sync # Better command that will try to optimise the performance for you!!!**
	- All high-level commands that involve uploading objects into S3 (aws s3 cp, aws s3 mv, and aws s3 sync) automatically perform a multipart upload when the object is large
- Parallel upload
	- each process uses a cpu
	- How you do this is very specific to the language (SDK)
	- In Python use multi-processing module
	- Can use threads (C++, Java)
		- Some tools already available:
	- s3-parallel-put (https://github.com/twpayne/s3-parallel-put )
- Random prefix to key name
	- first two characters of filename are part of the key, and should be randomised (evenly) by the alphabet so that they get put onto different partitions and hence the performance is much better
- tar
	 - can put everything into a tar archive (such as images), then look at the TOC of the tar and pull out only the byte range of the image you need from AWS
- Data compression

### Examples

#### SKA (square kilometer) array
- s3-parallel-upload script (python/boto2)
- performance dropped off after ~30 parallel processes

### HPC

#### Cluster HPC
- can do placement groups - which means the compute nodes are close to each other physically at the server farm
- storage is ephemeral, EBS, NFSv4 Amazon EFS or Lustre Intel cloud edition (PAYG), BeeGFS
- is still redundant within your region (so not as durable as S3)
- minimum use time on Amazon HPC is 1 hour (so cost is that of 1 hour)
 

#### Instance types

- Family - optimised for a particular use
	- ex R3 - memory
	- ex G2/P2 - GPU
	- ec C3/ CPU use type 
	- P2 
		- machine learning genomics (tensor flow)


#### Data lakes
- store datasets of different types 


# Research on AWS

# Lab: Alces Flight & CfnCluster


Apart from Alces flight, what other launch a cluster configurations are there?

In order to relaunch a snapshot you have you can use CfnCluster.

spark R machine learning

jupiter gnu  (ask him via email)

## If you need to use a key on Windows

[Puttylink](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)

