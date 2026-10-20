# Lab 0 — Amazon EC2 Foundations

**Program:** SEIR Foundations — Class 8  
**Module:** Week 1 · Module 1  
**AWS Region:** US East (N. Virginia) — `us-east-1`  
**Status:** Completed — four deployments terminated

## Overview

This lab introduced Amazon Elastic Compute Cloud (Amazon EC2) instance provisioning, remote Linux access, launch templates, deployment verification, and resource cleanup. I completed four EC2 deployments and documented each deployment's public IPv4 address, connection method, supporting screenshots, and termination status.

## Objectives

- Launch Amazon EC2 instances using a saved launch template.
- Connect to Linux instances using SSH with a `.pem` private key and browser-based EC2 Instance Connect.
- Verify the SEIR Foundations **Chewbacca Operations Center** dashboard.
- Inspect instance configuration and validate Linux access.
- Terminate lab instances and check for leftover storage and networking resources.
- Capture reproducible evidence for review.

## Environment and tools

| Component | Configuration |
| --- | --- |
| Cloud platform | Amazon Web Services (AWS) |
| Region | `us-east-1` |
| Operating system | Amazon Linux 2023 |
| EC2 instance types used | `t3.micro`, `t3.small` |
| Launch template | `seir-foundations-lab0` |
| SSH user | `ec2-user` |
| Connection tools | AWS EC2 Instance Connect; SSH through Git Bash / Visual Studio Code |
| Root storage in first deployment | 8 GiB `gp3` Amazon EBS volume |

## Deployment summary

| Deployment | Public IPv4 address at launch | Connection method | Terminated |
| --- | --- | --- | --- |
| 1 | `100.54.131.209` | SSH via VS Code / Git Bash using a `.pem` key | Yes |
| 2 | `50.16.43.120` | EC2 Instance Connect in the AWS Management Console | Yes |
| 3 | `100.63.174.255` | EC2 Instance Connect in the AWS Management Console | Yes |
| 4 | `3.93.193.104` | SSH via Git Bash using a `.pem` key | Yes |

The addresses above are historical deployment records; the instances have been terminated.

For the full deployment notes and screenshot references, see the [deployment tracking workbook](documentation/lab0_deployment_tracking.xlsx).

## Implementation and verification

### Deployment 1 — SSH Client (VS Code / Git Bash)

Public IPv4 Address: 100.54.131.209
Connection Method: SSH Client — VS Code/Git Bash with .pem key
Instance Terminated: Yes

I launched an Amazon EC2 t3.micro instance using my saved SEIR Foundations launch template in the us-east-1 Region. AWS assigned the public IPv4 address 100.54.131.209.

I connected to the instance using SSH from the Git Bash terminal in Visual Studio Code, authenticating with my .pem private key as ec2-user.

After connecting, the SEIR Foundations Chewbacca Operations Center dashboard displayed automatically. I also executed seir-dashboard manually to verify that the installation worked.

The dashboard showed 2 vCPUs, approximately 913 MiB of memory, an 8 GiB root disk, and system uptime. I used whoami and pwd to verify my Linux username and working directory.

During troubleshooting, I verified the SSH host-key fingerprint using the EC2 system log and used verbose SSH diagnostics to confirm successful public-key authentication.

### Deployment 2 — EC2 Instance Connect

Public IPv4 Address: 50.16.43.120
Connection Method: EC2 Instance Connect — AWS Console (browser-based SSH)
Instance Terminated: Yes

I deployed a second Amazon EC2 instance and connected using EC2 Instance Connect through the AWS Management Console. The instance was assigned public IPv4 address 50.16.43.120. After connecting, I confirmed that the SEIR Foundations Chewbacca Operations Center dashboard appeared successfully. I recorded the connection information and terminated the instance after completing the deployment.

### Deployment 3 — EC2 Instance Connect

Public IPv4 Address: 100.63.174.255
Connection Method: EC2 Instance Connect — AWS Console (browser-based SSH)
Instance Terminated: Yes

For my third deployment, I launched another Amazon EC2 instance and used the EC2 Instance Connect through the AWS Management Console. The instance received public IPv4 address 100.63.174.255. This deployment provided additional practice with the EC2 launch and connection process. I documented the connection method, recorded the required screenshot reference, and terminated the instance after completing the exercise.

### Deployment 4 — SSH Client (Git Bash)

Public IPv4 Address: 3.93.193.104
Connection Method: SSH Client — Git Bash with .pem key
Instance Terminated: Yes

I completed an additional fourth deployment to reinforce my ability to connect to Amazon EC2 instances using SSH. The instance was assigned public IPv4 address 3.93.193.104. I used Git Bash and my existing .pem private key to establish the SSH connection. This deployment reinforced my understanding of key-based authentication and remote Linux access. After documenting the deployment, I terminated the instance. 

## Evidence

- [Deployment tracking workbook](documentation/lab0_deployment_tracking.xlsx) — public IPv4 addresses, connection methods, screenshot references, short narratives, and termination status.
- [Screenshots folder](screenshots/) — saved console and terminal evidence from the four deployments.

> **Note:** Screenshot filenames in the workbook should match the files committed in `screenshots/`. Do not commit `.pem` private keys, AWS credentials, or other secrets.

## Resource cleanup and cost awareness

After completing the deployments, I terminated the EC2 instances. I also reviewed the AWS console for residual Amazon EBS volumes, owned snapshots, and allocated Elastic IP addresses. The screenshots captured during cleanup showed no remaining resources in those categories in the checked region.

The AWS billing review showed usage for EC2 instance runtime and public IPv4 addresses, offset by AWS Free Tier credits at the time of review. This reinforced that low CPU utilization does not necessarily mean a resource has no cost.

## Key lessons learned

1. A reusable launch template makes instance provisioning more consistent.
2. EC2 Instance Connect and SSH with a private key provide different ways to access Linux instances.
3. SSH connectivity depends on authentication, security group rules, and network reachability.
4. Verification should include both application behavior and basic Linux commands.
5. Terminating instances and reviewing related resources are important parts of cost-conscious lab work.
6. Organized screenshots and deployment records make technical work easier to reproduce and assess.

## Repository organization

```text
lab0_basics/
├── README.md
├── documentation/
│   └── lab0_deployment_tracking.xlsx
└── screenshots/
    └── [deployment screenshots]
```

---

**Lab result:** Four deployments documented, both connection approaches practiced, dashboard functionality verified, and instance cleanup completed.
