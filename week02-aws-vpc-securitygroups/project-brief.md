# Week 2 — AWS VPC + Security Groups (Project Brief)

Week 1 = got a static site working (basic).  
Now I want to try something bigger → split web + database.  

Main idea:  
- web server = public  
- database = private  
- only web server should talk to DB  

---

## Goal
- VPC with 2 subnets  
  - public subnet → EC2 web server (just Hello World again)  
  - private subnet → database (RDS if I can figure it out, or EC2 pretending to be one)  
- Internet Gateway → for public stuff  
- NAT Gateway → private subnet still needs updates  
- Security groups:  
  - Web SG → allow HTTP/HTTPS from anywhere  
  - DB SG → only traffic from Web SG  

---

## Questions / things I don’t fully get
- How does AWS actually stop the DB from being public?  
- Do SGs really block everything unless I open ports?  
- Internet Gateway vs NAT Gateway (still confusing, always mix them up 😅)  

---

## What “done” looks like
- I can open the web page from my browser  
- I can’t reach the DB directly from the internet  
- DB only responds to traffic from the web server  
- Draw a simple diagram at the end  

---

## Notes
Will probably mess up SG rules or route tables.  
That’s fine → I’ll fix it and hopefully remember better next time.  
This week is more about **networking basics**, not making something fancy.
