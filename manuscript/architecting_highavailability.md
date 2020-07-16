# Architecting for High-Availability on Alibaba Cloud
Highly available systems are reliable in the sense that they continue operating even when critical components fail. They are also resilient, meaning that they are able to simply handle failure without service disruption or data loss, and seamlessly recover from such failure. High availability is commonly measured as a percentage of uptime. The number of “nines” is commonly used to indicate the degree of high availability. For example, “four nines” is indicative of a system that is up 99.99% of the time, meaning it is down for only 52.6 minutes during an entire year.

High-Avalability is a qualitative measure which is affected by every component of a system, and by the way how this component is setup and how it is integrated with other components. As such, it is a broad topic that needs to be looked at at many different levels of abstractions and also has a great overlap with disaster recovery.

Alibaba Cloud provides different services for each level of abstraction: from workloads distribution on physical server level (deployment sets) to data centers and availability zones, to the built-in redundancy of basic services such as Service Load Balancers and VPN-Gateways, to sophisticated DNS-based load-balancing to account for cross-regional high-availabilty, and of course managed replication and synchronization services such as Data Transmission Service that allows for disaster recovery strategies for your database workloads.

Let us look into the various services and options in more detail.

## Regions and Availability Zones
Availability Zones (AZs) are distinct locations within an Alibaba Cloud Region that are engineered to be isolated from failures in other Availability Zones. They provide cost-free, low-latency network connectivity to other Availability Zones in the same Alibaba Cloud Region. Each region is completely independent. By launching your workloads in different AZs you are able to achieve the greatest possible fault tolerance.
More details on Alibaba Cloud's current regions and AZs can be found here: https://www.alibabacloud.com/help/doc-detail/40654.htm

Your workload distribution across availability zones directly influences the Availability Service Level Agreement (SLA). If your workload is only run on one ECS instance for example, your availability SLA will be 99,975% of monthly uptime. If your workload is deployed on at least 2 ECS instances across two or more AZs then your availability SLA will be 99,995%. You'll find detailed information on our services' SLA here: https://www.alibabacloud.com/help/doc-detail/42436.htm 

## Deployment Sets
https://www.alibabacloud.com/help/doc-detail/91258.htm

## Built-in High Availability of Alibaba Cloud Services 
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
