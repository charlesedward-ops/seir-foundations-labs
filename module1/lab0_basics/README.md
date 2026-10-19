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

### Deployment 1 — Launch and SSH validation

I launched an Amazon EC2 `t3.micro` instance from the saved SEIR Foundations launch template. I connected as `ec2-user` using SSH from Git Bash in Visual Studio Code, authenticating with my private `.pem` key. The Chewbacca Operations Center dashboard appeared automatically after login. I also ran `seir-dashboard` manually to confirm the command was installed and operational.

The dashboard displayed system information, including two vCPUs, approximately 913 MiB of memory, an 8 GiB root disk, and system uptime. I used `whoami` and `pwd` to verify my Linux user and working directory. During troubleshooting, I checked the SSH host-key fingerprint using the EC2 system log and used verbose SSH diagnostics to confirm public-key authentication.

### Deployment 2 — Browser-based access

I launched another EC2 instance and connected using **EC2 Instance Connect** in the AWS Management Console. The Chewbacca Operations Center dashboard appeared successfully. I recorded the deployment information and terminated the instance afterward.

### Deployment 3 — Repeatable deployment

I launched a third EC2 instance and connected through the AWS Management Console using EC2 Instance Connect. This deployment provided additional practice with provisioning and remote access. I captured the required evidence and terminated the instance.

### Deployment 4 — Repeat SSH authentication

I completed a fourth deployment to reinforce SSH access using Git Bash and my existing `.pem` private key. After connecting, I documented the deployment and terminated the instance.

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
