# AWS-VPC-project

## Objective

The Amazon VPC project aimed to create a secure, logically isolated network within AWS to understand and configure critical components like subnets, internet gateways, and route tables. This hands-on experience focused on deploying and managing AWS resources, configuring secure connections, and optimizing network security with security groups and network ACLs. The project was designed to enhance knowledge of cloud networking and security best practices within AWS.

### Skills Learned

- Proficient in creating and managing Amazon VPCs, subnets, and internet gateways.
- Deep understanding of CIDR block allocation and IP address management.
- Experience in configuring route tables and securing traffic flow.
- Knowledge of managing traffic using security groups and network ACLs for subnet and resource security.
- Developed skills in securing cloud environments by controlling inbound and outbound traffic at both the subnet and resource levels.

### Tools Used

- Amazon VPC for configuring and managing isolated cloud networks.
- Route tables for directing traffic within the VPC.
- Internet gateways for enabling internet connectivity for AWS resources.
- Security groups for controlling access to specific resources based on IP addresses, protocols, and ports.
- Network ACLs for subnet-level traffic control and security enforcement.

## Steps

![image](https://github.com/user-attachments/assets/8771cc96-9be2-4af6-bcde-635750728c44)


Introducing Today’s Project!
What is Amazon VPC?
Amazon VPC enables users to launch AWS resources within a logically isolated virtual network, providing complete control over their network environment and enabling secure on-premises-cloud hybridization.

Virtual Private Clouds (VPCs)
VPCs are isolated AWS cloud sections that help keep my AWS resources private and secure. There has already been a default VPC in my account ever since my AWS account was created. This is because AWS has set up a default VPC to allow me to deploy resources like EC2 instances/RDS databases immediately (without creating my own VPC). To set my VPC, I had to define an IPv4 CIDR, which means a range of IP addresses that my VPC can allocate to the resources deployed into my VPC.

![image](https://github.com/user-attachments/assets/a3f9cca0-34b0-42cd-85db-d7388c1e545e)



Subnets
Subnets are subsections of my VPC, just like how neighborhoods are subsections of a city There are already subnets in my account, one for every Availability Zone in the Region that I’ve set up my VPC in. Since my region is Virginia (US-east-1), which has six available Zones, I have six default subnets already. I named my subnet Deep 1, but that doesnʼt automatically make my subnet a public subnet. For a subnet to be considered public, it has to be considered public, it has to have a route and an internet gateway.

![image](https://github.com/user-attachments/assets/062f8270-ab4e-4365-909b-e9bdf6b9517d)



Internet gateways
Internet gateways are the key VPC components that allow internet access for the resources in my VPC/subnet. An internet gateway is also how users in the public can access my resources in a public subnet. Attaching an internet gateway means resources in your VPC can now access the internet. The EC2 instances with public IP addresses also become accessible to users, so your applications hosted on those servers become public too.

![image](https://github.com/user-attachments/assets/96c21cc9-cac4-4297-a3d5-790f2b704699)


What is a VPC?
When you create a new resource on AWS, these resources exist “somewhere on the cloud,” but you might wonder where exactly on the cloud they’re located.

AWS organizes every single resource created on its platform into Regions (the only exceptions to this are global resources, like IAM users and roles), and you can check which Region you’re in by looking at the top right-hand corner of your AWS Management Console.

If we imagine your AWS Region as a country, a Virtual Private Cloud (VPC) is like managing your city inside that country. You can design neighborhoods (i.e. subnets, which you’ll learn about in a second), traffic rules, and security measures to control how the different resources, like EC2 instances and S3 buckets, are connected and work together inside your VPC. Your city is isolated from other towns (i.e. other AWS Accounts’ VPCs), giving you full privacy and control over your VPC’s layout and rules.

![image](https://github.com/user-attachments/assets/1b268df9-247e-4060-85e9-f1501621ac6e)


💡 What is an IP address and why do I need to configure this?
An IP address is like a street address or coordinates for the resources in your city.

💡 What is a CIDR block?
CIDR is a way to assign a whole block of IP addresses, kind of like creating a zone/area in a city.

To understand how big a CIDR block is, look at the number after the slash — the smaller the number, the larger the CIDR block!

![image](https://github.com/user-attachments/assets/3f243e2c-62c3-4ba6-bf60-ef6eba73f543)


10.0.0.0/16 means the first 16 bits of your IP address (10.0) are fixed, but the remaining 16 bits (i.e. the second half of the IP address) can be allocated however you like. This means IP addresses within this CIDR block range from10.0.0.0to10.0.255.255! There are 2¹⁶ (65,536) possible IP addresses within this subnet.

A smaller CIDR block-like10.0.0.0/24 has the first 24 bits fixed, meaning only the last 8 bits can vary. This provides 2⁸ (256) possible addresses.

A larger CIDR block-like10.0.0.0/8 has only the first 8 bits fixed, allowing for a much larger number of varying bits — this gives you 2²⁴ (16,777,216) possible addresses.

💡 What is a route table?
Think of a route table as a GPS for the resources in your subnet. Just like a GPS helps people get to their destination in a city, a route table is a table of rules, called routes, that decide where the data in your network should go.

Every subnet in your VPC needs to be linked to a route table because the table tells the data where to travel. A subnet can only use one route table at a time, but multiple subnets can refer to the same route table as their GPS.

Without a route table, your resources wouldn’t know where to send or receive data! If you have a web server (i.e. an EC2 instance) hosting a website in your public subnet and you don’t have a route table, users cannot access your website because your subnet wouldn’t know where to take incoming traffic.

💡 What’s the link between internet gateways and route tables?
When a subnet’s route table has a route to an Internet gateway, the subnet becomes a public subnet, which means it can communicate with the Internet.

![image](https://github.com/user-attachments/assets/7541093f-0fe7-4d37-9a06-4968b914612f)


Route destination and target
Routes are defined by their destination and target, which implies that the destination is the range of IP addresses that traffic in my VPC is trying to reach. The target is the road/path that the traffic will use to get to their destinations.

The route in my route table that directed internet-bound traffic to my internet gateway had a destination of 0.0.0.0/0 and a target of my Classis IG (internet gateway).

💡 What is a security group?
If VPCs are cities and subnets are neighborhoods, a security group is a security checkpoint, or security guard, at the entrance for each building (resource) in that neighborhood (subnet).

Every resource must be associated with a security group. This means security groups don’t attach to a VPC or a subnet, they attach to a specific resource within that VPC/subnet. If you don’t specify a security group when you launch a resource, it will use the default security group that AWS creates whenever you set up a VPC.

Security groups are responsible for checking who comes in and out. They have strict rules about what kind of traffic can enter or leave the resource based on its IP address, protocols, and port numbers.

💡 Protocols and port numbers? What do they mean?

Protocols: With VPCs as our city and every resource as a building, think of protocols as different vehicles, like buses, taxis, and trucks, to deliver data in different ways. Protocols are special rules that help data move across the internet, each designed to send data for a specific kind of task. Here are some protocols you might come across:
HTTP (Hypertext Transfer Protocol): This is the standard protocol for sending web pages over the internet. Just like buses carry passengers, HTTP carries web page data to your browser.
SMTP (Simple Mail Transfer Protocol): SMTP is the standard protocol used for sending emails across the Internet! It acts as a mailman, ensuring that your email reaches the correct inbox without errors.
FTP (File Transfer Protocol): FTP is like a delivery truck that is used for transporting large loads of files between computers on a network. It’s a popular tool for uploading and downloading bulky items to and from servers, and developers often use FTP to upload website files directly to a hosting server from their local computers.
SSH (Secure Shell Protocol): SSH is like a secure, private car that allows encrypted communication directly with a server for private tasks such as managing software or configuring settings. In AWS and other cloud environments, SSH is widely used for securely accessing and managing virtual servers like EC2 instances! You’ll eventually come across SSH in more advanced projects.
Port numbers: Think of port numbers as specific doors or delivery docks on a building where data will enter or exit. Each port number is designed for a specific protocol, helping servers (i.e. EC2 instances) direct incoming and outgoing traffic to the right application/process that’s capable of processing that protocol. Without port numbers, an EC2 instance would struggle to manage incoming data correctly! It wouldn’t know which application or service each piece of data should be directed to.
Some common port numbers are:
Port 80: for HTTP protocols delivering web pages.
Port 25: for SMTP protocols delivering email.
Port 21: for FTP protocols delivering files

![image](https://github.com/user-attachments/assets/d669ece7-3237-4ae9-ba5d-f58725d6ae1c)


What’s the difference between inbound and outbound rules?
Inbound rules control the data that can enter the resources in your security group, while outbound rules control the data that your resources can send out. In this context, setting up inbound rules is important for allowing users to access your public website, while outbound rules help manage how your server interacts with other parts of the internet.

Examples of inbound data: Visitors to your website, your website receives form submissions.
Examples of outbound data: Your server requests data from another service, and sends out an email notification.
Create a Network ACL
That’s your traffic flow (route table) and basic security (security groups) sorted for your VPC!

To level up your VPC’s security, let’s add a network ACL i.e. network access control list.

💡 What are Network ACLs?
Think of Network ACLs as traffic cops stationed at every entry and exit point of your subnet, checking each data packet against a table of ACL rules before allowing them through.

💡 Data packets? What’s that?
When we talk about “traffic” moving through your VPC in AWS, we’re talking about data packets! For example, when a user types your website’s URL into their browser and presses enter, the “inbound traffic” entering your EC2 instance is data packets that represent your user’s request to see your website.

Data packets travel across the internet or within networks, and each packet contains a piece of the data being sent, e.g. the user’s request, or a part of a web page or a file. Network ACLs are responsible for inspecting and managing these data packets as they enter and exit your VPC’s subnets, making sure only authorized data moves in and out.

💡 What’s the difference between security groups and network ACLs?

Network ACLs set broad traffic rules that apply to an entire subnet. For example, blocking incoming traffic from a particular range of IP addresses or denying all outbound traffic to certain ports.
Security groups allow for more granular control, managing access to individual resources. You can specify which ports and protocols are allowed for each connected resource.
Having both is a great security practice! You can set broad restrictions at the subnet level with ACLs, and more specific limits at the resource level through security groups. This dual layer takes security to the next level as traffic must pass through multiple checks, which reduces the chances of unwanted access.

![image](https://github.com/user-attachments/assets/43f62a3b-0e8a-4653-b720-25054398f45e)


![image](https://github.com/user-attachments/assets/fedb97c7-e186-4bb6-bf73-4e99ce5451de)


What do the rules under the Inbound and Outbound rules tabs mean? Just like security groups, network ACLs use inbound and outbound rules to decide which data packets are allowed to enter or leave subnets:

Rule 100 Inbound allows all inbound traffic into the Public Subnet.
Rule 100 Outbound allows all traffic out of the Public Subnet.
The second line in each ruleset shows an asterisk (*) that acts as a catch-all rule in case traffic does not match any earlier rules. In our case, since Rule 100 already allows all traffic, the asterisk rule won’t come into play.

![image](https://github.com/user-attachments/assets/e23e6166-266b-4a76-a8fb-3ce6925fbf08)


Conclusion:

🚏 Set up route tables: Configure a route table in VPC to send Internet-bound traffic to your internet gateway, turning your subnet into a public subnet.
👮‍♀️ Implement security groups: created a security group to control inbound and outbound traffic at a resource level, specifying allowed IP addresses, protocols, and ports.
📋 Deploy network ACLs: set up network ACLs as an additional layer of security, managing both incoming and outgoing traffic at the subnet level.
