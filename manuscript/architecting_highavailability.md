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
*RDS* is short for *Relational Database Service* and it supports different relational database engines such as MySQL, Postgresql, SQL Server, MariaDB, PPAS (Oracle/EnterpriseDB). 
Below is a quick break-down and discussion of the available editions and configurations and how it may impact your availability Recovery Point Objective (RPO). 

RDS comes in four different editions:
- Enterprise <br>
The Enterprise Edition offers enterprise-level reliability with a Recovery Point Object (RPO) of 0, and supports the database engines MySQL 5.7 and 8.0.
It consists of one primary instance and two secondary instances. Your primary and secondary instances can be deployed in three different data centers in the same city to support cross-zone disaster recovery which makes it suitable for finance, securities, and insurance industries that require high data security. It comes with an availability SLA of 99,99% monthly uptime on dedicated instances. If deployed on general purpose instances the monthly availability SLA is 99,95%.
- High-Availability <br>
Your database system consists of one primary instance and one secondary instance. Data is synchronously replicated from the primary instance to the secondary instance. If the primary instance breaks down unexpectedly, your database system automatically fails over to the secondary instance. Secondary instance cannot be accessed. To scale horizontally for read operations you can add up to 10 read-replicas. It comes with an availability SLA of 99,99% monthly uptime on dedicated instances. If deployed on general purpose instances the monthly availability SLA is 99,95%.
- High-Performance (aka PolarDB) <br>
This edition is also known as PolarDB. A cloud-native database service which separates the compute and storage layer. It supports high scalability, large auto-incrementing storage space, low primary/secondary latency, and fault recovery wihtin several seconds. It allows you to expand the storage to up to 100 TB and scale out an individual cluster to up to 16 nodes. You can create a snapshot on a database of 2 TB in size within 60 seconds. The monthly availability SLA is 99,99%. It also comes with Global Database Replication feature that allows you to replicate data to read-only nodes in other regions including Mainland China over Alibaba Cloud's private backbone network.
- Basic <br>
The edition only provides a single master node and is not designed for high-availability. Its use-case is mainly for personal learning, small-sized websites, and development and test environments for small- and medium-sized enterprises. 

RDS supports two different instance types:
- General Purpose
A general-purpose instance exclusively occupies the memory resources allocated to it, but shares CPU and storage resources with the other general-purpose instances that are deployed on the same server.
CPU resources are moderately reused among general-purpose instances that are deployed on the same server to increase CPU cost-effectiveness. The same configuration might lead to higher compute and storage performance compared to a dedicated instance. It is not, however, consistent over a longer period of time.
- Dedicated
A dedicated instance exclusively occupies the CPU and memory resources allocated to it. Its performance remains stable for a long term and is not affected by the other instances that are deployed on the same server.

RDS supports two different deployment methods:
- Single-Zone Deployment <br>
Indicates that the primary and secondary instances are located in the same zone. Replication may be faster, but zone faults will have a serious impact on the entire database setup.
- Multi-Zone Deplyoment <br>
Indicates that the primary and secondary instances are located in different zones for cross-zone disaster recovery. Replication is slower, but the database setup is resilient against individual zone faults. Note that this mode is currently not supported by all regions.

RDS supports different storage types:
- Local SSD (available for Enterprise Edition and High Availability only) <br>
This is the recommended storage type. A local SSD resides on the same server as the database engine and therefore reduces I/O latency. The maximum possible size is directly related to the RDS instance being used. Upgrades of instance types may take very long time, however, since data might be needed to get copied over to a new physical database server in case the original one does not have enough capacity. Both logical and physical backups are supported.
- Enhanced SSD (available for High-Availability) <br>
It is also a recommended storage type. This new SSD product is designed by Alibaba Cloud based on next-generation distributed block storage architecture. It integrates 25 Gigabit Ethernet and remote direct memory access (RDMA) technologies to provide super high performance at low latency. An enhanced SSD can process up to 1 million random read/write requests per second. Depending on the DB engine and instance type is suports up to 32 TB storage capacity.
- Standard SSD (available for High-Availability and Basic) <br>
A standard SSD is an elastic block storage device that is designed based on a distributed storage architecture. You can store data on a standard SSD to separate computing from storage. It is cheaper than ESSD but does not provide the high performance characteristics.
- High-Performance Distributed Storage (available for High-Performance) <br>
Sharing the same group of data copies among multiple DB servers, rather than storing a separate copy of data for each DB server, significantly reduces your storage cost. The distributed storage and file system allows automatically scaling up database storage capacity, regardless of the storage capacity of each single database server. This enables your database to handle up to 100 TB of data at max. Storage capacity is bound by an instance-specific soft-limit which can be increased via ticket, however. See https://www.alibabacloud.com/help/doc-detail/68498.htm for details.

For every storage option except *Local SSD*, backups are done as snapshots including the binary log to guarantee consistency.

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
