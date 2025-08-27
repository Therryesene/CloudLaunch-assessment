# CLOUDLAUNCH AWS INFRASTRUCTURE PROJECT 
This project focuses on the implementation of CloudLaunch which is a lightweight platform using AWS Core services such as S3, IAM, and VPC.
# Task 1: Static Website Hosting Using S3 + IAM User with Limited Permissions
# S3 Buckets Configurations.
Three different S3 buckets with different access levels were created with the region being Europe (Ireland)

1. Cloudlaunch-myawsbucket
  - This bucket was confirgured for static website hosting.
  - It is also made publicly accessible by turning off the "Block all public access" option.
  - The bucket contains HTML and CSS resources for the CloudLaunch website
  - Website URL: http://cloudlaunch-myawsbucket.s3-website-eu-west-1.amazonaws.com
2. Cloudlaunch-privateawsbucket
  - Not publicly accessible.
  - The user has access to the Object ownership.
3. Cloudlaunch-visible-only-mybucket
  - Not publicly accessible
  - Read only access
# IAM USER AND POLICY
 Created policies:
  - Cloudaccesspolicy
  - CloudLaunchVpcPolicy
An IAM USER account was created with limited permission to the S3 buckets created.
# IAM Policy
## {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": [
                "arn:aws:s3:::cloudlaunch-myawsbucket",
                "arn:aws:s3:::cloudlaunch-privateawsbucket",
                "arn:aws:s3:::cloudlaunch-visible-only-mybucket"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::cloudlaunch-privateawsbucket/*"
        },
        {
            "Effect": "Allow",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::cloudlaunch-myawsbucket/*"
        }
    ]
}
# TASK 2: VPC Design for CloudLaunch Environment
## VPC Configuration
  - VPC Name: Cloudlaunch-vpc
  - CIDR Block: 10.0.0.0/16
  - Region: eu-west-1a
# Subnet Configuration

## Public Subnet
  - Name: cloudlaunch-public-subnet
  - CIDR: 10.0.1.0/24
  - Avalibility zone: euw1-az2 (eu-west-1a)
  - Purpose: Intended for load balancers or future public-facing services.
## Application Subnet
  - Name: cloudlaunch-app-subnet
  - CIDR: 10.0.2.0/24
  - Avalibility zone: euw1-az2 (eu-west-1a)
  - Purpose: Intended for app servers (private).
## Database Subnet
  - Name: cloudlaunch-db-subnet
  - CIDR: 10.0.3.0/28
  - Avalibility zone: euw1-az2 (eu-west-1a)
  - Purpose:  Intended for RDS-like services (private).
# INTERNET GATEWAY
  - Name: Cloudlaunch-gateway
  - IGW ID: igw-0252a014ba0ee620c
  - Status: Connected to Cloudlaunch-vpc
# ROUTE TABLES
  - Public Route Table (cloudlaunch-public-rt)
     - Routes:
     0.0.0.0/0 (IGW)
     10.0.0.0/16 (LOCAL)
  - Application Route Table (cloudlaunch-app-rt)
     - Routes:
     10.0.0.0/16 (Local)
  - Database Route Table (cloudlaunch-db-rt)
     - 10.0.0.0/16 (Local)
# SECURITY GROUPS
  - cloudlaunch-db-sg
  - cloudlaunch-app-sg

# AWS CONSOLE SET UP
  - AWS Console URL: https://eu-north-1.signin.aws.amazon.com/oauth?
  - Access Type: AWS Management Console access only

# User Credentials Setup
 - Console Password: Temporary password requiring change on first login
 - Password Requirements: Minimum 8 characters, mixed case, numbers, symbols
 - Password Reset: Should be reset upon initial login
 
