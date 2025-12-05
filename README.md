1. NETWORKING AND SUBNETTING (AWS VPC setup) TASK
   
I created one VPC with the CIDR block 10.0.0.0/16 to provide a large private IP range for future scalability. Inside this VPC, I designed four subnets—two public and two private—each placed in different Availability Zones for high availability.The public subnets were assigned CIDR blocks 10.0.1.0/24 and 10.0.2.0/24, which gives enough IPs for internet-facing resources like load balancers or NAT gateways. I attached an Internet Gateway (IGW) to the VPC and updated the public route table to route all outbound traffic (0.0.0.0/0) through the IGW, making these subnets publicly accessible.For internal workloads, I created two private subnets with CIDR blocks 10.0.3.0/24 and 10.0.4.0/24. These subnets do not have direct internet access for security reasons. Instead, I created a NAT Gateway inside one of the public subnets and configured the private route table to send outbound traffic to the NAT Gateway. This allows resources in private subnets (like EC2, RDS) to access the internet securely for updates or package installation without exposing them publicly.The final architecture ensures proper network segmentation, secure outbound access for private resources, and internet connectivity for public subnets according to AWS best practices. 

2. EC2 WEBSITE HOSTING TASK

I launched a Free Tier EC2 instance (t2.micro) inside a public subnet so that it could be accessed from the internet. I assigned it a public IP and configured the security group to allow only the required traffic—HTTP (port 80) from anywhere and SSH (port 22) restricted to my own IP for secure access.After launching the instance, I connected to it using AWS EC2 Instance Connect. I installed Nginx, which acts as a lightweight web server for hosting static pages. Once Nginx was running, I uploaded my resume (static HTML/CSS) to the default Nginx directory (/usr/share/nginx/html/). This allowed the resume website to load whenever someone accessed the instance’s public IP.For basic infrastructure hardening, I restricted SSH access, removed unnecessary packages, enabled automatic updates, and ensured that only port 80 was publicly exposed. This improves security while keeping the website accessible.When the setup was complete, opening the public IP in a browser displayed my resume as a static website served by Nginx.

4. BILLING AND FREE TIER COST MONITORING TASK
   
a) WHY COST MONITORING IS IMPORTANT FOR BEGINNERS:

Cost monitoring is important for beginners because AWS services continue running unless they are manually stopped or deleted. New users often forget to shut down instances, leaving resources active and generating charges. By enabling billing alerts early, beginners can track their spending, avoid unexpected bills, and stay within the Free Tier limits. It also helps build good cloud cost-management habits from the start.

b) WHAT CAUSES SUDDEN INCREASES IN AWS BILLS:

Sudden AWS bill increases usually happen when unused resources continue running in the background. Common causes include:
Forgetting to stop or terminate EC2 instances
Running NAT Gateways or Load Balancers, which have hourly and data processing costs
Storing large data in S3 or exceeding free storage limits
Leaving EBS volumes, snapshots, or Elastic IPs unused
Using services not covered by the Free Tier
High data transfer charges due to large outbound traffic
These unexpected charges can be avoided by enabling cost alerts and regularly reviewing billing dashboards.
