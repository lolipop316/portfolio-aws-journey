# AWS Secure Network Components — ALB + Bastion + Private Instance  
### Week 03 — Security+ 4.2 / Network Components Hardening

### Summary
This phase extended the previous two-tier (web → database) design into a secure three-tier network.  
The goal was to deploy an **Application Load Balancer (ALB)** and a **Bastion Host** to enforce segmented access between public and private resources.  
All traffic was restricted through chained Security Groups, and administrative access was managed with **AWS Systems Manager (SSM)** instead of SSH keys.

---

### Architecture Overview
- **VPC ID:** `vpc-0ff99c91f32a361ea`
- **Subnets**
  - `lab-public` → Bastion Host + ALB  
  - `lab-private` → Web Instance (private-web)
- **Routing**
  - Public RTB → Internet Gateway `igw-015951ecfd3371291`
  - Private RTB → Local routes only
- **Access Control**
  - Bastion SSH from my IP only (`/32`)
  - ALB open on port 80 to public
  - Private instance accepts HTTP from ALB SG and SSH from Bastion SG
- **Administration**
  - SSM Session Manager used for secure access (no key pair exposure)

![VPC Overview](./labvpc_reuse_20251024.png)

---

### Evidence

| # | Description | Screenshot |
|:-:|:--|:--|
| 01 | VPC routes showing IGW for public subnet and local for private | ![vpc_routes_20251024](./vpc_routes_20251024.png) |
| 02 | Bastion host running in public subnet | ![bastion_details_20251024](./bastion_details_20251024.png) |
| 03 | Bastion security group restricts SSH to my IP only | ![bastion_sg_rules_20251024](./bastion_sg_rules_20251024.png) |
| 04 | Secure SSM session to bastion (no SSH keys) | ![bastion_session_manager_20251024](./bastion_session_manager_20251024.png) |
| 05 | Private web instance (no public IP) running in private subnet | ![private_instance_20251024](./private_instance_20251024.png) |
| 06 | Private SG allows HTTP from ALB and SSH from Bastion | ![private_sg_rules_20251024](./private_sg_rules_20251024.png) |
| 07 | Verified SSH chain (bastion → private EC2) and metadata access | ![ssh_chain_success_20251025](./ssh_chain_success_20251025.png) |
| 08 | ALB overview showing status Active and listener HTTP :80 | ![alb_overview_20251024](./alb_overview_20251024.png) |
| 09 | ALB security group inbound rule allowing HTTP from internet | ![alb_sg_rules_20251024](./alb_sg_rules_20251024.png) |
| 10 | Target group with healthy private instance registered | ![alb_target_health_20251024](./alb_target_health_20251024.png) |
| 11 | HTTP test through ALB DNS endpoint returning 200 OK | ![alb_http_test_20251024](./alb_http_test_20251024.png) |

---

### Reflection
This lab completed the secure-access chain for a realistic cloud network.  
All management and application traffic is now segmented:
- Public users → ALB → Private EC2  
- Administrators → Bastion → Private EC2 (via SSM)

Using SSM instead of SSH keys eliminated key rotation risks.  
Security Groups and subnet boundaries worked together to enforce least-privilege access.  
The HTTP 200 response verified that ALB and target health checks were operating correctly.

---

### Next Steps
- Add HTTPS with ACM certificates and redirect rules.  
- Integrate CloudWatch alarms and WAF ACL for monitoring.  
- Automate deployment with Terraform and policy-as-code guardrails.  
- Extend IAM roles for EC2 and implement KMS for encryption in transit and at rest.

---

### Verification Summary

| Test | Command / Evidence | Result |
|:--|:--|:--|
| ALB Connectivity | `curl -I http://sp4-alb-454520712.ap-southeast-2.elb.amazonaws.com` | ✅ HTTP 200 OK |
| Bastion Access | SSM Session Manager login as `ssm-user` | ✅ Connected securely |
| Private Isolation | No public IP + SG restricted to ALB and Bastion | ✅ Verified |
| Target Health | ALB Target Group → Healthy | ✅ Verified |
| Routing | Public via IGW / Private local only | ✅ Verified |

---

### Outcome
A fully functional, secure three-tier VPC architecture demonstrating Network Segmentation, Zero Trust principles, and AWS native security best practices.  
This work aligns with **CompTIA Security+ Domain 4.2: Secure Network Components** and builds the foundation for Terraform and AWS Security Specialty automation labs.
