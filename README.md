<h2 align="center"> CREATING VPC, SUBNET, IGW AND LINKING THEM UP</h2>

In this project, I am going to demonstrate how to create a VPC, Subnets, Route Tables, Security Groups, and EC2 instances and link them up. This project is to simulate an organization's network, with two subnets. The first subnet, the public subnet (a bastion host), represents a public server hosting an EC2 instance, and the second is the private subnet hosting the organization's internal application, which is not public facing. This is a basic form of a 3-tier architecture in AWS. Below is the network architecture.

<p align="center">
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/Network-architecture.png" height="70%" width="80%"/>
</p>


<h3 align="left"> Creating a Virtual Private Cloud (VPC)</h3>

The first step is to create the VPC (private cloud network) of the organization. From services, I navigated to VPC and created a new VPC named Home-Lab.

<p>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/creating-a-vpc.png" height="60%" width="40%"/>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/creating-a-vpc2.png" height="40%" width="50%"/>
</p>

<h3 align="left"> Creating a Private and Public Subnet</h3>

On the vpc dashboard under the virtual private cloud on the left pane, I clicked on subnet, then create subnet on the right pane. I proceeded to create the two subnets (private and public), selecting the VPC I previously created (Home-lab). 

<p>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/subnet1.png" height="60%" width="40%"/>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/subnet2.png" height="60%" width="40%"/>
</p>

<h3 align="left"> Creating the Routing Tables</h3>

When a VPC is created, a new routing table is automatically created. I renamed the created routing table Public_RT which would be used for the public subnet and created a new routing table (Private_RT) for the private subnet. 

<p align="center">
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/private-rt.png" height="60%" width="40%"/>
</p>

<h3 align="left"> Creating Internet Gateway</h3>

For the public subnet to communicate with the internet, I created an internet gateway, which I will later attach to the vpc. 

<p>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/igw.png" height="60%" width="40%"/>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/attaching-igw-to-vpc.png" height="60%" width="50%"/>
</p>
 
<h3 align="left"> Linking up Everything</h3>

First I went to the Public RT, under the route tab, I edited the route table to route traffic from all IP addresses through the internet gateway. Under the subnet associations tab, I edited the subnet association and linked the public subnet to the public rt. 

<p>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/configuring-public-rt-to-igw.png" height="50%" width="45%"/>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/linking-public-subnet-public-rt.png" height="50%" width="45%"/>
</p>

Then I went to the Private RT, under the subnet association tab, I edited the subnet association and linked the private subnet to the private rt.

<p align="center">
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/linking-private-subnet-private-rt.png" height="40%" width="60%"/>
</p>

<h3 align="left"> Configuring Security Groups</h3>

Whenever a new VPC is created, a new security group is also created. Security at this level is referred to as security at instance level. First I renamed the created security group (HL_SG ), edited the inbound rules to allow all traffic, white-listing my IP address. Another thing that is automatically created after a vpc is created is the Network Access Control Lists (NACLs), which is also referred to as security at network level. I renamed and allowed all traffic white-listing my IP address.

<p align= "left">
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/editing-sg.png" height="50%" width="70%"/>
</p>

<p align= "left">
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/editing-acl.png" height="50%" width="70%"/>
</p>

<h3 align="left"> Creating EC2 Instances in Both Subnets</h3>

I created EC2 instances for both the Public Subnet and Private Subnet, I already had a key pair I previously created which I used for the required key pair. In the network settings, I selected the vpc, the right subnet for each type of instance and the security group. One way to confirm that I am using the correct security group is to compare the VPC IDs.

<p>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/public-ec2-1.png" height="60%" width="40%"/>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/public-ec2-2.png" height="60%" width="40%"/>
</p>

<p>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/private-ec2-1.png" height="45%" width="55%"/>
<img src="https://github.com/Topzdomain/Creating-VPC-Subnet-Internet-Gateway-Route-Tables-and-Linking-Them-up/blob/main/screenshots/private-ec2-2.png" height="45%" width="40%"/>
</p>

<h3 align="left"> Connecting to the EC2 Instances</h3>

The public ec2 has internet access, so login into the public server can be achieved using SSH.

```commandline
ssh -i wazuh.pem ubuntu@3.83.142.219
```
To access the private server (private ec2 instance) in the private subnet, there is a need to ssh from the public server (the bastion host) into the private server. To do this, I copied the key pair (wazuh.pem) which was the key pair associated with both ec2 instances into the public server, after which I was able to ssh into the private server using ssh and the key pair. Also, I used the private IP address of the private server and not the public address.
On my Windows command line

```commandline
scp -i wazuh.pem wazuh.pem ubuntu@3.83.142.219:/home/ubuntu/"wazuh.pem"
```
```commandline
sudo chmod u=rw,g= ,o= wazuh.pem
ssh -i wazuh.pem ubuntu@192.168.2.239
```commandline

<h3 align="left"> Challenges</h3>

Although I was able to SSH into the public ec2 instance, the instance could not ping Google (www.google.com). What I did was to re-configure the NACLs to allow all traffic while traffic is only allowed from my IP address on the security group. Since the private instance was not connected to the internet, the instance could not be accessed. 
