To create a Private Network Environment in AWS, follow these steps:

Configure Security Group:

Search for VPC in the search bar in your AWS account.
Rename the Default VPC to something of your choice.
In the Next step, go to the EC2 Dashboard and click on "Security Groups
Click on "Create Security Group"
In the Configure Security Group" section, configure the security group to allow SSH access (port 22) from your IP address.
You may also want to allow other ports based on your requirements (e.g., HTTP, HTTPS) as shown in inbound-rules-ports.png file in PHASE-1 directory.

Inbound rules Info

HTTP
Inbound rule 1 Type
TCP
80

Custom
Inbound rule 1 Source
0.0.0.0/0

Delete
sgr-0f30ed5d2cfc21530

SMTPS
Inbound rule 2 Type
TCP
465

Custom
Inbound rule 2 Source
0.0.0.0/0

Delete
sgr-049e831b6645e2deb

SMTP
Inbound rule 3 Type
TCP
25

Custom
Inbound rule 3 Source
0.0.0.0/0

Delete
sgr-0e67a654bd34ffe01

Custom TCP
Inbound rule 4 Type
TCP
6443

Custom
Inbound rule 4 Source
0.0.0.0/0

Delete
sgr-07df5852b459846fc

HTTPS
Inbound rule 5 Type
TCP
443

Custom
Inbound rule 5 Source
0.0.0.0/0

Delete
sgr-0f5eada212abfcec9

Custom TCP
Inbound rule 6 Type
TCP
30000 - 32767

Custom
Inbound rule 6 Source
0.0.0.0/0

Delete
sgr-08f0fc8449f5c8f7f

SSH
Inbound rule 7 Type
TCP
22

Custom
Inbound rule 7 Source
0.0.0.0/0

Delete
sgr-04b7ec45f262e0db9

Custom TCP
Inbound rule 8 Type
TCP
3000 - 10000

Custom
Inbound rule 8 Source
0.0.0.0/0

  
