# Highly Available Web Application on AWS

## Architecture
- ALB → Auto Scaling EC2 (Multi-AZ)
- RDS (Primary + Secondary)

## Components
- ALB
- EC2 Auto Scaling
- RDS
- Security Groups
- IAM Roles

## Verification Steps
1. ALB health check
2. EC2 auto-scaling test
3. DB connectivity test
4. Failover simulation

## Notes
- RDS deployed in single AZ for implementation
- Diagram shows multi-AZ design


𝐏𝐫𝐨𝐣𝐞𝐜𝐭 𝟏: 𝐇𝐢𝐠𝐡𝐥𝐲 𝐀𝐯𝐚𝐢𝐥𝐚𝐛𝐥𝐞 𝐖𝐞𝐛 𝐀𝐩𝐩𝐥𝐢𝐜𝐚𝐭𝐢𝐨𝐧 𝐨𝐧 𝐀𝐖𝐒
Recently, I designed and implemented a Highly Available Web Application architecture on AWS, focusing on fault tolerance and scalability using AWS best practices.
𝗣𝗿𝗼𝗯𝗹𝗲𝗺
Single instance deployments are vulnerable to downtime caused by instance failures or traffic spikes.
𝗦𝗼𝗹𝘂𝘁𝗶𝗼𝗻:
-Implemented Multi AZ EC2 deployment with Auto Scaling for web servers (used default VPC)
1.     Created a launch template (added user data to visually confirm load balancing in browser).
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Highly Available App - $(hostname)</h1>" > /var/www/html/index.html
2.     Created target groups to receive traffic from ALB 
3.     Created auto scaling Group using Launch template , enabled  ELB health checks with scaling policy of min: 2 and max:4
-Used Application Load Balancer (ALB) for distributing traffic
1.     Created ALB with at least 2 Availability zones and attached Security Group created for ALB.
- Designed a highly available architecture with RDS Multi-AZ.                                                                                                                       Implemented Single-AZ RDS during hands-on deployment due to AWS Free Tier constraints.
-Secured architecture with Security Groups: 
1.     Security group for ALB with inbound HTTP (80)-0.0.0.0/0 and outbound as All Traffic which allows public traffic to ALB.
2.     Security Group for EC2 with inbound as ALB security group (HTTP (80)), SSH (22) my ip , outbound as All Traffic which ensures only ALB can talk to EC2
3.     Security group for RDS: inbound as  EC2 security group (MySQL (3306))and outbound as All Traffic which ensures database is accessible only from app layer.
𝗩𝗲𝗿𝗶𝗳𝗶𝗰𝗮𝘁𝗶𝗼𝗻 𝘀𝘁𝗲𝗽𝘀:
1.     Confirmed ALB traffic distribution by refreshing ALB DNS hostname in browser which may show different EC2 names.
2.     Verified EC2 Auto Scaling Group by comparing desired capacity and running instances.
3.     Verified EC2 to RDS connection by SSH to one instance and ran mysql -h <RDS-ENDPOINT> -u admin -p
4.     Verified fault tolerance by terminating one EC2 instance . ASG launched a new instance and app remains accessible via ALB.
5.     Verified ALB health checks by stopping one instance which routes traffic to other instance.
6.     Verified accessibility to each resource to ensure the rules defined in security groups are working as expected.
