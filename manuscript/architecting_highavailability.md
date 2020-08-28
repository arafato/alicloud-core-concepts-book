# Architecting for High-Availability on Alibaba Cloud
Highly available systems are reliable in the sense that they continue operating even when critical components fail. They are also resilient, meaning that they are able to simply handle failure without service disruption or data loss, and seamlessly recover from such failure. High availability is commonly measured as a percentage of uptime. The number of “nines” is commonly used to indicate the degree of high availability. For example, “four nines” is indicative of a system that is up 99.99% of the time, meaning it is down for only 52.6 minutes during an entire year.

High-Avalability is a qualitative measure which is affected by every component of a system, and by the way how this component is setup and how it is integrated with other components. As such, it is a broad topic that needs to be looked at at many different levels of abstractions and also has a great overlap with disaster recovery.

Alibaba Cloud provides different services for each level of abstraction: from workloads distribution on physical server level (deployment sets) to data centers and availability zones, to the built-in redundancy of basic services such as Service Load Balancers and VPN-Gateways, to sophisticated DNS-based load-balancing to account for cross-regional high-availabilty, and of course managed replication and synchronization services such as Data Transmission Service that allows for disaster recovery strategies for your database workloads.

Let us look into the various services and options in more detail.

## Regions and Availability Zones
Availability Zones (AZs) are distinct locations within an Alibaba Cloud Region that are engineered to be isolated from failures in other Availability Zones. They provide cost-free, low-latency network connectivity to other Availability Zones in the same Alibaba Cloud Region. Each region is completely independent. By launching your workloads in different AZs you are able to achieve the greatest possible fault tolerance. More details on Alibaba Cloud's current regions and AZs can be found here: https://www.alibabacloud.com/help/doc-detail/40654.htm

Your workload distribution across availability zones directly influences the Availability Service Level Agreement (SLA). If your workload is only run on one ECS instance for example, your availability SLA will be 99,975% of monthly uptime. If your workload is deployed on at least 2 ECS instances across two or more AZs then your availability SLA will be 99,995%. You'll find detailed information on our services' SLA here: https://www.alibabacloud.com/help/doc-detail/42436.htm 

## Deployment Sets
A deployment set is a policy that controls the distribution of ECS instances on the physcial server hosts. As of now, it supports the so-called *High-Availability* policy. If it used, all the ECS instances within your deployment set are strictly distributed across different physical servers within the specified region. The high availability policy applies to application architectures where several ECS instances must be isolated from each other. The policy significantly reduces the chances of service being unavailable. When you create ECS instances in a deployment set, you can create up to seven ECS instances in each zone. This limit varies with your ECS usage. You can use the following formula to calculate the number of ECS instances that can be created in an Alibaba Cloud region: *7 × Number of availability zones*. Deployment sets do not incur any additional costs. For more details please consult: https://www.alibabacloud.com/help/doc-detail/91258.htm

## Built-in Redundancy of Alibaba Cloud Services 
Many of the services on Alibaba Cloud provide built-in redundancy out of the box to provide a high degree of high-availability and according SLAs. We will quickly discuss the most commonly used ones here:
### Service Load Balancer (SLB)
Server Load Balancer (SLB) is a traffic distribution and control service that distributes inbound traffic among several backend servers, namely ECS instances, based on configured forwarding rules and works both on TCP/UDP and HTTP(S) level. SLB expands the serving capacity of applications and enhances their availability.

Deployed in clusters, SLB can synchronize sessions among node servers to protect the SLB system from single points of failure (SPOFs). This improves redundancy and guarantees service stability. Layer-4 SLB uses the open source software Linux Virtual Server (LVS) and Keepalived to achieve load balancing. Layer-7 SLB uses Tengine to achieve load balancing. Tengine, a Web server project based on Nginx, adds advanced features dedicated for high-traffic websites.

Requests from the Internet reach the LVS cluster through Equal-Cost Multi-Path (ECMP) routing. Each LVS in the LVS cluster synchronizes the session to other LVS machines through multicast packets, thereby implementing session synchronization among machines in the LVS cluster. At the same time, the LVS cluster performs health checks on the Tengine cluster and removes abnormal machines to guarantee the availability of layer-7 SLB.

An SLB is always deployed in two AZs. If a primary zone becomes unavailable, SLB rapidly switches to a secondary zone to restore its service capabilities within 30 seconds. When the primary zone becomes available, SLB automatically switches back to the primary zone. The Availability SLA is a monthly uptime of at least 99,99%.

Please note that the layer 7 SLB is deployed as a cluster of usually 8 Tengine nodes (see https://tengine.taobao.org/ for details on Tengine). This has some very important implications for the maximum number of *Queryies per Secone (QPS)* a certain SLB instance can sustain. For example, an *slb.s1.small* can sustain 1000 QPS at max. These 1000 QPS refer to the overall cluster limit, not an individual node of the cluster! So in case of persistent HTTP connections (which is the default setting starting with HTTP 1.1) the requests are always send to the same node. Thus, for a particular client you may need to divide the overall QPS limit by the number of nodes (which is usually 8). So for an *slb.s1.small* this would result in 1000 / 8 = 125 QPS per node.

### VPN-Gateway
VPN Gateway is an Internet-based service that securely and reliably connects enterprise data centers, office networks, or Internet-facing terminals to Alibaba Cloud Virtual Private Cloud (VPC) networks through encrypted connections. VPN Gateway supports both IPsec-VPN connection and SSL-VPN connection. By design, an instance of VPN-Gateway is redundantly setup, meaning there is an invisible failover instance in case the primary goes down.

For configuring redundant IPSec connections with both an *Active* and a *Standby* tunnel the local gatewy needs two public IP addresses. Be sure to configure the healthcheck on the VPN-Gateway as well and then define according weight values. 100 for the active tunnel and 0 for the standby tunnel. When the active tunnel is unavailable all traffic between the on-premises data center and the VPC is then automatically directed to the standby tunnel. 

### RDS and PolarDB
For relational databases workloads Alibaba Cloud provides two types of services:
- RDS for MySQL, Postgresql, SQL Server, MariaDB, PPAS (Oracle/EnterpriseDB)



- PolarDB for for MySQL, Postgresql, Oracle


### Object Storage Service (OSS)


### Cloud Enterprise Network (CEN)



SLB, VPNGW, RDS, etc
https://www.alibabacloud.com/help/doc-detail/67915.htm


## Multi-Site High Availability and Cross-Region Load Balancing
https://www.alibabacloud.com/help/doc-detail/87478.htm
https://www.alibabacloud.com/help/doc-detail/87477.htm

(Cross-Region, Cross-DataCenter) Data Synchronization and Replication with DTS
https://www.alibabacloud.com/help/doc-detail/26598.htm


## Disaster Recovery
In addition to before-mentioned services such as DTS we also do provide dedicated services for disaster recovery strategies of VM-based workloads (hybrid, cloud-to-cloud, account-to-account) such as HBR, and are supported by dedicated 3rd party offerings such as Hystax for example.

## Hybrid Backup Recovery (HBR)
https://www.alibabacloud.com/help/doc-detail/62362.htm

## 3rd party Integration
https://hystax.com/disaster-recovery-to-alibaba/
