VPC: Virtual Private Cloud
=================

VPC 101
-----------
* One of the most important topics
* Think of Virtual Data Center in the cloud
    * Logically isolated section of AWS resources
    * Default VPC is created for each account
        * Once deleted cannot be undone without contacting Amazon - do not delete unless strictly required
* Hardware VPC
    * Hybrid Cloud - AWS as extension of existing corporate data center
* VPC Peering
    * Direct connection between VPCs
    * No transitive VPC Peering, i.e. if
        * VPC 1 peered to VPC 2
        * VPC 2 peered to VPC 3
        * VPC 1 cannot talk to VPC 3 without direct peering
        * Think Star Schema

VPC Lab
-----------
* Create VPC
    * CIDR: Classless Inter-Domain Routing
        * 10.0.0.0/16
        * 8bits.8bits.8bits.8bits = 32bits
        * 2 ^ (32 - 16) = 2 ^ 16 = 65536 addresses available with this mask
    * Tenancy
        * Default
            * On shared hardware
        * Dedicated
            * All resources will be on dedicated hardware, e.g. EC2
    * Route table created automatically when VPC is created
* Create Subnets
    * Always mapped to one AZ, cannot be across multiple AZ. One Subnet = One AZ
    * Route table was created by default
* Create Internet Gateway
    * Only one Internet Gateway per VPC
    * Detached by default
        * Attach to VPC
* Create New Route Table
    * Because we want IGW to be able to access internet
    * Inside our new Route table
        * Add route
            * Allow all traffic (0.0.0.0/0) to our Internet Gateway
    * Associate with a subnet which requires Internet access
* Launch EC2 instance
    * Public Subnet 1 with Internet
    * Can ssh into this instance
* Launch EC2 instance 
    * Private Subnet 2 without Internet
    * Single Security Group can be applied across different Subnets (AZ)
    * Cannot directly ssh into this instance from Internet or update system from Internet
    * Can ssh into this instance from another EC2 instance in Subnet 1 using private ip, e.g. `ssh ec2-user@10.0.1.0 -i key.pem`

NAT: Network Address Translation
------------
* As a separate EC2 instance with Internet access
    * NAT is used as a bridge to provide EC2 instances in private subnets with outgoing internet connection
    * Based on public Amazon NAT AMI
    * Disable "Source/Destination" check
        * All EC2 instances do Source/Destination checks to either send or receive traffic
        * But this, of course, is not required for NAT instance because 
            it needs to be able to send and receive traffic on behalf of EC2s in private subnets
    * May need a larger EC2 instance which comes with better network latency/throughput
* Create new security group for NAT instance
    * Allow ICMP traffic for "ping google.com" tests
    * Allow incoming and outgoing HTTP/HTTPS traffic  
        * May do for specific IP range or EC2 instances
* Enable EC2 instances in private subnet to talk to NAT instance
    * Go to default/main 

NACL: Network Access Control List
------------

* Acts like Firewall/iptables
* Default NACL allows all traffic
* Rules
    * Numbered
    * Evaluation from lowest to highest
    * Add rules in multiples of 100 to be able to insert rules later where required
    