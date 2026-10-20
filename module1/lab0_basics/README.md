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

I launched an Amazon EC2 t3.micro instance using my saved SEIR Foundations launch template in the us-east-1 Region. AWS assigned the public IPv4 address 100.54.131.209. I connected to the instance using SSH from the Git Bash terminal in Visual Studio Code, authenticating with my .pem private key as ec2-user. After connecting, the SEIR Foundations Chewbacca Operations Center dashboard displayed automatically. I also executed seir-dashboard manually to verify that the installation worked. The dashboard showed 2 vCPUs, approximately 913 MiB of memory, an 8 GiB root disk, and system uptime. I used whoami and pwd to verify my Linux username and working directory. During troubleshooting, I verified the SSH host-key fingerprint using the EC2 system log and used verbose SSH diagnostics to confirm successful public-key authentication.

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

## Reflection and Lessons Learned

1. What CPU, memory, disk, and uptime tell me about the instance?

During my first deployment, the SEIR Foundations Chewbacca Operations Center dashboard displayed 2 virtual CPUs (vCPUs), approximately 913 MiB of memory, an 8 GiB root disk, and system uptime.

These metrics helped me understand the instance's available computing resources and operational status:

- **CPU:** Indicates the instance's processing capacity and, when monitoring utilization, how much processing power is being used.

- **Memory:** Shows the RAM available to run applications and system processes. Monitoring memory helps identify potential resource limitations.

- **Disk:** Shows the instance's storage capacity and usage, helping determine how much space is available for the operating system, applications, and files.

- **Uptime:** Indicates how long the operating system has been running since its last startup or reboot. This helps verify that the instance is operational and provides information about its running history.

Reviewing these metrics reinforced the importance of monitoring system resources when managing cloud infrastructure.

2. What became easier after repeating the process?

After completing four EC2 deployments, I became more comfortable with the overall instance launch, connection, verification, and termination process.

The most noticeable improvement was my ability to establish SSH connections using Git Bash and my .pem private key. Repeating the process helped me better understand the relationship between the EC2 public IPv4 address, Linux username (ec2-user), private key authentication, and security group settings.

I also gained experience using two connection methods: SSH through Git Bash and browser-based EC2 Instance Connect.

By the fourth deployment, I had a clearer understanding of the sequence of steps required to launch an instance, establish remote access, verify the environment, document the results, and clean up AWS resources.

3. What errors did I encounter, and how did you resolve them?

During the first deployment, I encountered challenges establishing and verifying my SSH connection.

I used verbose SSH diagnostics to investigate the connection process and confirm successful public-key authentication. I also verified the instance's SSH host-key fingerprint against information available in the EC2 system log.

These troubleshooting steps helped me distinguish between connection security verification and authentication issues.

After successfully connecting, I verified the Linux environment using whoami and pwd. I also confirmed that the SEIR Foundations Chewbacca Operations Center dashboard launched automatically and could be executed manually using seir-dashboard.

This experience reinforced the importance of reading terminal output, using diagnostic commands, and systematically verifying each stage of a connection rather than relying on trial and error.

4. Did I retain the launch template and clean up your practice instances?

Yes. I retained my saved SEIR Foundations EC2 launch template so it can be reused for future labs and deployments.

After completing the four deployments, I terminated the practice EC2 instances to prevent unnecessary ongoing compute charges.

I also reviewed the AWS EC2 console to verify that the instances were terminated and checked for remaining EBS volumes, snapshots, and Elastic IP addresses.

The resource checks confirmed that no EBS volumes, snapshots, or Elastic IPs remained in the selected AWS Region.

I also reviewed AWS Billing and Cost Management to better understand the charges associated with EC2 compute and public IPv4 address usage.

This reinforced an important cloud operations principle: infrastructure management includes not only deploying and verifying resources, but also monitoring costs and cleaning up resources that are no longer needed.

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
