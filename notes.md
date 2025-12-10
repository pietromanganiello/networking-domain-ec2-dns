# Networking Fundamentals — Applied Notes

This document explains the key networking concepts that were applied during the Domain + EC2 + DNS project and how they worked together in a real-world setup.

---

## Public IPv4 Addressing

The EC2 instance was assigned a public IPv4 address by AWS. This address allows the server to be reachable from anywhere on the internet. The public IP represents the unique destination that external devices use to send traffic to the server.

When a user enters the website in a browser, traffic is routed across the internet to this public IP address.

---

## DNS and A Records

DNS (Domain Name System) is responsible for translating human-readable domain names into IP addresses.

An A record was created in Cloudflare to map:

pietrodevops.uk → 51.21.202.133

This allows users to visit the website using the domain name instead of the raw IP address. When a browser queries DNS, it receives the IP address and then sends traffic to the EC2 instance.

---

## Routing and Internet Access

Once DNS resolves the domain to the EC2 public IP, the request is routed across the public internet. Internet routers forward packets based on routing tables until the traffic reaches the AWS network where the EC2 instance is hosted.

AWS then routes the traffic internally to the correct virtual network interface attached to the EC2 server.

---

## AWS Security Groups as Firewalls

A security group was configured to control inbound network traffic:

Port 22 (SSH) was allowed for secure remote administration  
Port 80 (HTTP) was allowed to serve the web page publicly  

All other ports were blocked by default. This behaves as a virtual firewall at the network level and prevents unauthorised access.

---

## TCP and Ports

HTTP traffic uses TCP on port 80. When a user accesses the website, their browser opens a TCP connection to port 80 on the server.

SSH access uses TCP on port 22. This port allows secure terminal access for server administration.

Ports act as logical doorways that allow multiple services to run on the same IP address.

---

## NGINX as the Web Server

NGINX was installed on the EC2 instance to serve web content. Once started, it listens for incoming TCP connections on port 80.

When a browser sends an HTTP request, NGINX processes the request and returns the default welcome page to the client.

---

## End-to-End Traffic Flow Summary

User enters pietrodevops.uk in a browser  
DNS resolves the domain to the EC2 public IP  
Traffic is routed across the internet to AWS  
AWS security group checks port 80 and allows the connection  
NGINX receives the request and returns the webpage  

---

## Why This Matters for DevOps and Cloud Engineering

Understanding how DNS, IP addressing, routing, firewalls, and web servers interact is fundamental to building reliable cloud infrastructure.

This project demonstrates the exact same network flow used in real production systems, just at a smaller scale.
