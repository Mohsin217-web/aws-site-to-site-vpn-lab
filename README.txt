AWS SITE-TO-SITE VPN LAB
========================

Project:
AWS Site-to-Site VPN with pfSense, Transit Gateway and Private EC2

AWS Region:
ap-south-1


PROJECT OVERVIEW
================

This project demonstrates a hybrid network architecture connecting an
on-premises pfSense firewall to an AWS VPC using AWS Site-to-Site IPsec VPN
and AWS Transit Gateway.

The lab validates end-to-end private connectivity between the on-premises
network and a private EC2 instance inside AWS.

Architecture:

On-Premises pfSense
        |
        | IPsec Site-to-Site VPN
        |
AWS Site-to-Site VPN
        |
        | VPN Attachment
        |
AWS Transit Gateway
        |
        | VPC Attachment
        |
AWS VPC
        |
Private Subnet
        |
Private EC2


NETWORK TOPOLOGY
================

ON-PREMISES NETWORK

LAN CIDR:
192.168.20.0/22

pfSense LAN IP:
192.168.20.1

pfSense VPN Public IP:
110.38.255.78


AWS NETWORK

VPC:
aws-vpn-vpc

VPC CIDR:
10.50.0.0/16

Private Subnet:
aws-vpn-private-subnet

Subnet CIDR:
10.50.1.0/24

Availability Zone:
ap-south-1a


EC2

Name:
aws-vpn-test-ec2

Private IP:
10.50.1.194

Instance Type:
t3.micro

Operating System:
Ubuntu


TRANSIT GATEWAY
===============

Transit Gateway:
tgw-02eaab1bbcc604939

VPC Attachment:
tgw-attach-02e534ce3c3c0e97f

VPN Attachment:
tgw-attach-0dc3b0bd73d852b9d

Dedicated TGW Route Table:

Name:
aws-vpn-tgw-rt

Route Table ID:
tgw-rtb-0bf3507f7aa502a7b


SITE-TO-SITE VPN
================

VPN Connection:
vpn-03797fbbefa7b769f

Customer Gateway:
cgw-0949341409982eaf9

Customer Gateway Public IP:
110.38.255.78

Routing:
Static

VPN Type:
IPsec.1

Authentication:
Pre-Shared Key


VPN TUNNELS
===========

Tunnel 1:
AWS Outside IP: 13.126.13.147
Inside CIDR: 169.254.243.60/30
Status during validation: UP

Tunnel 2:
AWS Outside IP: 13.204.119.254
Inside CIDR: 169.254.136.124/30
Status during validation: DOWN

Note:
The lab was successfully validated using Tunnel 1.
Tunnel 2 high-availability configuration was not completed.


IPSEC TRAFFIC SELECTORS
=======================

Local Network:
192.168.20.0/22

Remote Network:
10.50.0.0/16

Protocol:
ESP

Phase 1:
IKEv1
AES128
SHA1
DH Group 2

Phase 2:
AES128
SHA1
PFS Group 2
Lifetime 3600 seconds


TRANSIT GATEWAY ROUTING
=======================

Dedicated TGW Route Table:

10.50.0.0/16
    -> VPC Attachment

192.168.20.0/22
    -> VPN Attachment


VPC ROUTING
===========

AWS VPC Route Table:

10.50.0.0/16
    -> local

192.168.20.0/22
    -> Transit Gateway


CONNECTIVITY VALIDATION
=======================

Test:

Source:
pfSense 192.168.20.1

Destination:
EC2 10.50.1.194

Result:

5 packets transmitted
5 packets received
0.0% packet loss

Average latency:
approximately 46.755 ms

Status:
SUCCESS


VALIDATED TRAFFIC FLOW
======================

Office Network
192.168.20.0/22
        |
        v
pfSense
192.168.20.1
        |
        v
IPsec Site-to-Site VPN
        |
        v
AWS Transit Gateway
        |
        v
AWS VPC
10.50.0.0/16
        |
        v
Private Subnet
10.50.1.0/24
        |
        v
EC2
10.50.1.194


KEY LEARNING OUTCOMES
=====================

- Configured AWS Site-to-Site IPsec VPN with pfSense
- Configured a Customer Gateway
- Used AWS Transit Gateway as the VPN termination point
- Created a dedicated Transit Gateway route table
- Configured VPN and VPC route propagation
- Configured static routing for the on-premises CIDR
- Configured AWS VPC routing toward the Transit Gateway
- Configured pfSense IPsec Phase 1 and Phase 2
- Validated end-to-end connectivity between on-premises and AWS
- Troubleshot IPsec tunnel status and AWS routing
- Verified connectivity with ICMP testing


EVIDENCE
========

Screenshots included in the project:

01-pfsense-ipsec-tunnel-established.png
02-pfsense-ipsec-spd.png
03-pfsense-ipsec-sad.png
04-tgw-vpc-attachment.png
05-tgw-vpn-attachment.png
06-tgw-vpn-route-table.png
07-tgw-vpc-propagation.png
08-vpc-route-to-tgw.png
09-ec2-private-instance.png
10-pfsense-to-aws-ping-success.png


SECURITY NOTE
=============

No private keys, AWS credentials, VPN pre-shared keys, certificates,
passwords, or other secrets should be committed to this repository.

The AWS VPN configuration file containing sensitive parameters must remain
private and must not be uploaded to GitHub.


PROJECT STATUS
==============

Lab connectivity:
SUCCESS

Tunnel 1:
UP

Tunnel 2:
DOWN

End-to-end connectivity:
VERIFIED

Cleanup:
AWS lab resources were removed after validation.
