# AWS VPC + NAT Gateway (Phase 1)

This is my first proper AWS networking project.  
Last week I just got a static site running — this time I wanted to go a bit deeper and understand how AWS networks actually work.

---

### What I was trying to do

I built a basic two-tier network:
- One **public subnet** for a web server
- One **private subnet** for a database
- An **Internet Gateway** so public resources can be reached
- A **NAT Gateway** so private resources can still get software updates

It’s not about fancy apps yet — just about making sure the architecture is right and that I understand how things connect.

---

### Network setup

**VPC CIDR:** `10.0.0.0/16`  
**Public Subnet:** `10.0.1.0/24`  
**Private Subnet:** `10.0.2.0/24`

Resources I created:
- `lab-vpc`
- `lab-igw` (Internet Gateway)
- `lab-nat` (NAT Gateway with Elastic IP `13.55.91.167`)
- `lab-rtb-public` → routes to IGW
- `lab-rtb-private` → routes to NAT GW
- `lab-public` subnet (for web)
- `lab-private` subnet (for database)

---

### What I got working

 The VPC was created and both subnets are online  
 IGW attached and working  
 NAT Gateway active with Elastic IP  
 Route tables linked to the right subnets  
 Private subnet can reach the internet (through NAT)  
 Public subnet has open outbound/inbound internet  
 All screenshots saved for documentation  

---

### Screenshots

- `nat-lab_vpc_overview.png`
- `subnets_public.png`
- `subnets_private.png`
- `igw_attached.png`
- `nat_gateway_available.png`
- `rtb_public.png`
- `rtb_private.png`
- `vpc_nat_architecture.png`

---

### What I learned

- The Internet Gateway is what actually connects the VPC to the internet.  
- NAT Gateway only allows **outbound** traffic — it’s one-way protection for private subnets.  
- Private subnets stay isolated unless you route through the NAT.  
- Public and private subnets use different route tables — that’s what makes the separation work.  
- Once I saw the route tables line up correctly, the whole thing finally clicked.

---

### Next up

In **Phase 2**, I’ll add:
- EC2 web server (in public subnet)
- Database (in private subnet)
- Security groups to make sure only the web server can talk to the DB

That’s when I’ll test actual traffic between the two.
