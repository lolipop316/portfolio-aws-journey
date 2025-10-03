# Project Brief — Week 2: Secure Two-Tier AWS Architecture

## Goal
After getting my static site working in Week 1, I wanted to step up slightly and figure out how to separate a web server from a database in AWS.  
The main idea is: web stuff should be public, databases should not. I wanted to see how AWS makes that work with VPCs, subnets, and security groups.

## Architecture Overview
- VPC with two subnets:
  - Public Subnet → EC2 web server (just a “Hello World” page on Apache).  
  - Private Subnet → database (I’ll use RDS if I can, otherwise just a mock EC2 DB).  
- Internet Gateway → lets the public subnet actually be public.  
- NAT Gateway → private subnet can still fetch updates without being exposed.  
- Security Groups:  
  - Web SG → inbound 80/443 from anywhere.  
  - DB SG → only allow inbound from the Web SG.  
  - Block everything else by default.  

(CIDR blocks I used: 10.0.1.0/24 for public, 10.0.2.0/24 for private.)

## What I Want to Learn
- How AWS actually separates public and private resources.  
- How Security Groups enforce “least privilege.”  
- The difference between Internet Gateway and NAT Gateway (I mixed these up at first).  

## Success Criteria
- I can hit the web server from my browser.  
- I **cannot** reach the database directly from the internet.  
- The database only responds to the web server’s SG.  
- Diagram is created to show the final design.  

## Reflection
Compared to Week 1, this isn’t about “make a site live” — it’s more about planning the network properly. I expect to get confused a bit around routing/NAT, but that’s the whole point of doing it this early.  
