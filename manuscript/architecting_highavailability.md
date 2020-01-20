# Architecting for High-Availability on Alibaba Cloud

Really broad topic that needs to be looked at at many different levels and also has great overlap with disaster recovery.
Bottom line here is, that we provide different services for each level of abstraction: from workloads distribution on physical server level (deployment sets) to DCs (availability zones), to SaaS offerings for testing and assessing HA of your application ad architecture (AHAS), to the built-in redundancy of basic services such as SLB, to sophisticated DNS-based load-balancing to account for cross-regional high-availabilty, and of course managed replication and synchronization services such as DTS that allows for disaster recovery strategies for your DB workloads

Application High Availability Service (AHAS)
https://www.alibabacloud.com/help/doc-detail/90320.htm

Availability Zones
https://www.alibabacloud.com/help/doc-detail/40654.htm

High Availability of SLB
https://www.alibabacloud.com/help/doc-detail/67915.htm

Deployment Sets
https://www.alibabacloud.com/help/doc-detail/91258.htm

Multi-Site High Availability and Cross-Region Load Balancing
https://www.alibabacloud.com/help/doc-detail/87478.htm
https://www.alibabacloud.com/help/doc-detail/87477.htm

(Cross-Region, Cross-DataCenter) Data Synchronization and Replication with DTS
https://www.alibabacloud.com/help/doc-detail/26598.htm


** Disaster Recovery
In addition to before-mentioned services such as DTS we also do provide dedicated services for disaster recovery strategies of VM-based workloads (hybrid, cloud-to-cloud, account-to-account) such as HBR, and are supported by dedicated 3rd party offerings such as Hystax for example.

Hybrid Backup Recovery (HBR)
https://www.alibabacloud.com/help/doc-detail/62362.htm

3rd party Integration
https://hystax.com/disaster-recovery-to-alibaba/
