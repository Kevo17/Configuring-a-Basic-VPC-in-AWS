<h1>Configuring a Custom Domain with Cognito</h1>


<h2>Description</h2>
In this hands-on lab scenario, you’re a cloud network engineer tasked with setting up the security and network architecture for your organization's production environment. You'll have the opportunity to explore and understand the relationship between networking components. We will create a virtual private cloud (VPC), subnets across multiple availability zones (AZs), routes, and an internet gateway, as well as adding security using security groups and network access control lists (NACLs). These services are the foundation of networking architecture inside of AWS, and this lab will cover concepts such as infrastructure, design, routing, and security.
<br />

<h2>Environments Used </h2>

- <b>Windows 11</b>
- <b>AWS</b>
  
<h2>Program walk-through:</h2>

Navigate to VPC and create a new VPC. 
On the Create VPC page, fill in the following fields:

- Resources to create: Select VPC Only
- Name tag - optional: Type in HoLVPC.
- IPv4 CIDR block: Select IPv4 CIDR manual input.
- IPv4 CIDR: Type 10.0.0.0/16.
- IPv6 CIDR block: Select No IPv6 CIDR block.
- Tenancy: Select Default.<br />
<br />
Scroll down and click Create VPC
<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

On the Subnets page, click Create subnet.
<br/>
On the Create subnet page, under VPC ID, click into the empty field, and then choose the one (HoLVPC) option.
<br/>
Under the Subnet settings, set the following fields:
- Subnet Name: Type hol-public-a
- Availability Zone: Select us-east-1a
- IPv4 CIDR block: Type 10.0.1.0/24
- Below the Subnet settings, click Add new subnet.

<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Under Subnet 2 of 2, set the following fields:

- Subnet Name: Type hol-private-b
- Availability Zone: Select us-east-1b
- IPv4 CIDR block: Type 10.0.2.0/24
- At the bottom of the page, click Create Subnet


<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

- On the Subnets page, click the checkbox next to hol-public-a subnet to select it.
- In the top right-hand corner of the page, click Actions > Edit subnet settings.
- On the Edit subnet settings page, under Auto-assign IP settings, click the checkbox next to Enable auto-assign public IPv4 address.
- Click Save at the bottom of the page

<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

- From the Subnets page, in the left-hand menu, select Internet gateways.
- On the Internet gateways page, click Create internet gateway in the top right.
- On the Create internet gateway page, under Name tag, type hol-VPCIGW.
- Click Create internet gateway at the bottom of the page.

<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

- From the internet gateway's page, in the top right, click on Actions > Attach to VPC.
- On the Attach to VPC page, click into the empty field under Available VPCs.
- Choose the one option that says - HoLVPC at the end.
- Click Attach internet gateway at the bottom of the page.

<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

- Navigate to Create route table
- On the Create route table page, under Name - optional, type the name publicRT.
- Under VPC, click into the empty field, and then select the one (HoLVPC) option.
- Click Create route table at the bottom of the page.

<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

- On the route table's page, halfway down the page, on the Routes tab, click Edit routes on the right.
- On the Edit routes page, click Add Route.
- For the new route, under Destination, type 0.0.0.0/0.
- Under Target, select Internet Gateway, and then click again on the igw- that ends with (hol-VPCIGW).
- Click Save changes.

<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

- Navigate to the Subnet associations tab.
- On the Subnet associations tab, in the Explicit subnet associations section, click Edit subnet associations.
- Click the checkbox next to the hol-public-a subnet.
- Click Save associations


<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

1. Navigate to EC2 and launc a new Instance.
2. On the Launch an instance page, in the name field, type hol-pub-instance.
3. In the Application and OS Images section, make sure, by default, that Amazon Linux 2 is selected for the Amazon Machine Image (AMI) and make sure the Architecture is set to 64-bit (x86).
4. In the Instance Type section, select t3.micro from the dropdown.
5. In the Key pair (login) section, click Create new key pair.
6. In the Create key pair window, for the Key pair name, type vpcpubhol.
7 Leave the Key pair type set to RSA.
8. For the private key file format, choose either .pem or .ppk, depending on if you will use PuTTY or OpenSSH to connect to the EC2 instance later on.
- Note: Use .pem for a mac device; use .ppk for a Windows device.
9. Click Create key pair.
10. Back on the Launch an instance page, in the Network settings section, click Edit.
11. Make sure that, by default, the VPC is set to a vpc- that ends in (HoLVPC), and the Subnet is set to the  hol-public-a subnet.
12. Under Security group name, change the name, by typing in holpubSG.
13. Under Description - required, type holpubSG.
14. Under Security Group rule 1, set the following fields:
- Type: Select ssh
- Source type: My IP
15. Under Source, take note of the IP address, as you will need it for the last objective. Copy and paste it to a text editor to keep it handy for later in the lab.
16. Click Add security group rule.
17. Under Security Group rule 2, set the following fields:
- Type: Select ssh
- Source type: Select Custom
- Source: Type 10.0.0.0/16
18. Leave all other settings as the default, and click Launch instance.
19. Note: Your vpcpubhol .pem or .ppk key pair file should have downloaded. You will use this file later in the lab.
20. Go back to the Instances section of the EC2 dashboard.
21. On the Instances page, hit the refresh icon at the top of the screen to check the progress of the hol-pub-instance.



<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

1. From the Instances page, click Launch instances in the top right.
2. On the Launch an instance page, in the Name field, type hol-priv-instance.
3. In the Application and OS Images section, make sure, by default, that Amazon Linux 2 is selected for the Amazon Machine Image (AMI) and make sure the Architecture is set to 64-bit (x86).
4. In the Instance Type section, select t3.micro from the dropdown.
5. In the Key pair (login) section, click Create new key pair.
6. In the Create key pair window, in the Key pair name, type vpcprivhol.
7. Leave the Key pair type set to RSA.
8. For the Private key file format, choose either .pem or .ppk, depending on if you will use PuTTY or OpenSSH to connect to the EC2 instance later on.
- Note: Use .pem for a mac device; use .ppk for a Windows device.
9. Click Create key pair.
10. Back on the Launch an instance page, in the Network settings section, click Edit.
11. Make sure that, by default, the VPC is set to a vpc- that ends in (HoLVPC), and the Subnet is set to the hol-private-b subnet.
12. Under Security group name, change the name by typing in holprivSG.
13. Under Description - required, type holprivSG.
14. Under Security Group rule 1, set the following fields:
- Type: Select ssh
- Source type: Select Custom
- Source: Type 10.0.0.0/16
15. Leave all other settings as the default, and click Launch instance.
- Note: Your vpcprivhol .pem or .ppk key pair file should have downloaded. You will use this file later in the lab.
16. Once your instance has launched, click View all instances.
17. On the Instances page, hit the refresh icon to check the progress of the hol-priv-instance.

<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Ssh into the hol-pub-instance
<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

On the public instance, create a private key, as that will be used for the SSH connection. Run vi vpcprivhol.pem
<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Paste the contents of the vpcprivhol.pem into the file and enter :wq
<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Run chmod 400 vpcprivhol.pem to ensure your key is not publicly viewable
<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Copy the private IPv4 address of the instanceof the hol-priv-instance
<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Back in the terminal, run ssh -i "vpcprivhol.pem" ec2-user@<PRIVATE IP> making sure to replace <PRIVATE IP> with the IP address you just copied
<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

1. Navigate to the VPC console and select Network ACLs. 
2. Edit the inbound rules of the default Network ACL.
3. Click Add new rule and set the following values for the new rule:
- Rule number: Type 50
- Type: Select All traffic
- Source: Paste in the IP address you copy and pasted into a text editor earlier in the lab (Or you can get it by googling What is my IP in a new browser tab), and append /32 at the end
- Allow/Deny: Select Deny
5. Click Save changes.

<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

In your SSH terminal, navigate back to Downloads directory, then try to connect and log in to your public EC2 instance using the same command that you used to log in earlier.
You should receive an error.


<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

1. Return to the AWS console, and on the Network ACLs page, click Actions > Edit inbound rules.
2. On the Edit inbound rules page, click Remove next to rule 50.
3. Click Save changes.

<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Return to your SSH client, and try again to connect and log in to your public EC2 instance, using the same command you just used.<br/>

You should be able to connect now.

<br/>
 
<p align="center">
<img src="https://i.imgur.com/zvvF6rZ.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />
