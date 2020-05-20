# Infrastructure Essentials
This chapter gives a focused and very condensed rundown on the very essentials on the Alibaba Cloud Infrastructure service concepts such as Compute, Network, and Storage. Almost like a cheat-sheet.

## Elastic Compute Service (ECS)
ECS is the IaaS-service by Alibaba Cloud that provides customers compute power as virtual machines. The security and compliance is a shared responsibility between Alibaba Cloud and the customers.
Alibaba Cloud is responsible for "Security of the Cloud". That is, it is responsible for protecting the infrastructure that runs all of the services offered in the Alibaba Cloud. This infrastructure is composed of the hardware, software, networking, and facilities that run Alibaba Cloud services. For ECS, Alibaba Cloud's responsibility includes everything up to the hypervisor of the host machines that powers the ECS service. Everything above is the customer's responsibility which includes the guest operating system and all security configuration tasks such as configuration of Alibaba Cloud provided firewall (called a security group) on each network interface of an ECS instance. 

### Availability Service Level Agreement (SLA)
ECS comes with a Monthly Single-Instance Availability SLA of 99,975%, and provides a Monthly Multi-Zone Availability SLa of 99,995% if your application is deployed on at least 2 ECS instances spread across two different Availability Zones.

An ECS instance is considered *Unavailable* if the disconnection between an ECS instance configured with access permitted rules and any IP address over TCP or UDP in the inbound and outbound directions lasts for more than one minute.  

### Instance Families and Instance Types
The ECS service is organized by so-called *instance families* which in turn consist of different *instance types*.
An instance family describes the fundamental characteristics and use-cases for instances types of this family. Some are optimized for network-intense applications, others are desigend for memory or compute-intense workloads.
As such they usually differ in terms of Core-to-RAM ratio, maximum persistent disk IOPS, network performance (as both in bandwidth and PPS), and the type of disks they support. Please consult the official documentation at https://www.alibabacloud.com/help/doc-detail/25378.htm for detailed numbers.

Note that ECS configurations and types can be updated, however, not arbitrarily. For instance family and type changes only certain types are supported (see below documentation link). As a rule of thumb, you can always change instance families between g6, c6, and r6. 

As of this writing there exist 13 different instance families on Alibaba Cloud (please consult documentation at https://www.alibabacloud.com/help/doc-detail/108490.htm for details). The character in brackets denotes the abbreviation of the instance family.
- General Purpose (g)
- Compute Optimized (c)
- Memory Optimized (r)
- Big Data (d)
- Local SSDs (i)
- High-Clock Speed (hfc)
- Compute Optimized with GPU (gn, vgn)
- Visualization Compute with GPU (ga)
- Compute Optimized with FPGA (f)
- Bare Metal  (ebmg)
- Super Computing Cluster (scc)
- Burstable (t)
- Dedicated Hosts (ddh)

The new generations of Alibaba Cloud x86-based ECS instances are equipped 2.5 GHz Intel ® Xeon ® Platinum 8269CY (Cascade Lake) processors with Turbo Boost up to 3.2 GHz. The newest generation of the General Purpose G family now also support burstable network bandwidth. Also note that the 6th generations of g, c, r families are now running on Alibaba Cloud X-Dragon hypervisor architecture which provides predictable and consistent high performance and reduces virtualization overheads.

For our GPU-based instance types, we currently provide the NVIDIA® Tesla® P4 with the 1/8, 1/4, 1/2, and 1/1 computing capacity support, and the NVIDIA T4 GPU with 16 GB memory capacity (320 GB/s bandwidth),
2,560 CUDA Cores, up to 320 Turing Tensor Cores, and mixed-precision Tensor Cores support for 65 FP16 TFLOPS, 130 INT8 TOPS, and 260 INT4 TOPS. 

Deployment Sets (https://www.alibabacloud.com/help/doc-detail/91258.htm) gives you control on the distribution strategy. You can use a deployment set to distribute your ECS instances to different physical servers to guarantee high availability and set up underlying disaster discovery. When you create ECS instances in a deployment set, Alibaba Cloud will start the instances on different physical servers within the specified region based on your configured deployment strategy. Right now, Alibaba Cloud only provides "High Availability" strategy. As an effect all the ECS instances within your deployment set are strictly distributed across different physical servers within the specified region. The high availability strategy applies to application architectures where several ECS instances need to be isolated from each other. The strategy significantly reduces the chances of services becoming unavailable.

Each and every ECS instance has one operating system disk and can have up to 16 data disks each 32TB in size at max.  

### Dedicated Hosts (DDH)
ECS instances as discussed in the previous section share the underlying physical servers with different tenants. For scenarios that require strict isolation of the underlying resources DDH is a specialized solution for enterprise customers. DDH provides a dedicated hosting environment for a single tenant based on the virtualization technology of Alibaba Cloud. It offers flexible and scalable services that enable you to enjoy exclusive use of all resources provided by a physical server. 

A very useful feature for cost-efficiency is the ability to over-provision a DDH. This is only possible with certain DDH instance types such as the `ddh.v5`. It allows you to provision 336 vCPUS on a 48 core machine. This way you can fully utilize your compute capacity in case your workloads have a lot of idle time. See https://www.alibabacloud.com/help/doc-detail/68564.htm for a complete list of all DDH instance types and their respective specifications.

In case the physical machine is malfunctioning failover to a healthy instance is managed automatically by Alibaba Cloud. A new healthy DDH will be assigned automatically from the shared pool to the your account, and after the migration is done, the crashed DDH will be removed from your account. The DDH ID will remain the same after the migration, the machine ID will be different, though. Note that automatic failover is not supported for DDHs with local storage (i.e. `ddh.i2`).

### System Events and Live Migration
In previous sections we already talked about the *Shared Responsibility Model* of Alibaba Cloud. Sometimes, maintenance activities on our services such as ECS executed by Alibaba Cloud also affect your system. Thus, in order to react to and handle such kinds of events gracefully you need a way to get notified and possibly define appropriate actions. Please welcome, [ECS System Events](https://www.alibabacloud.com/help/doc-detail/66574.htm).

A system event is a scheduled and recorded maintenance event of ECS service. System events occur when updates, invalid operations, unexpected system failures, or unexpected hardware or software failures are detected on your ECS instance. Moreover, you will receive notification about the details of the event in the console when it occurs, including the event response plan and event cycle.

When an ECS user receives a notification from Alibaba Cloud, he or she can acknowledge the planned underlying maintenance for ECS instance by system event. The user can then choose the appropriate time window to execute the system event as well as operation activities according to individual business needs. By providing users this flexibility, users can reduce the impact on system reliability and business continuity.

Normally, when there is maintenance activity planned on the physical server, the ECS instance will be live migrated to another server to maintain the health of ECS instance with minimal performance impact. Note, that data stored on local SSDs (also sometimes referred to as ephemeral disks) is no migrated so make sure that your application accounts for that.

The official documentation at https://www.alibabacloud.com/help/doc-detail/66574.htm gives a deep-dive on the various system event types and event statuses, and also explains how to modify scheduled restart times of ECS instances.

For example, to view the all *unsettled events* (i.e. scheduled events that have not yet been executed) you can easily grab them via the CLI as follows:
```
aliyun ecs DescribeInstancesFullStatus --RegionId <TheRegionId> --InstanceId.1 <YourInstanceId> --output cols=EventId,EventTypeName
```
## Storage
This section will give a short overview about the fundamental aspects of storage on Alibaba Cloud. In particular, we will focus on storage services that you can use in combination with ECS, and Object Storage.

### ECS Mountable Storage
Generally speaking, there exist two kinds of storage options you can use in combination with ECS instances:
1. Persistent Storage
2. Ephemeral Storage

Persistent Storage keeps your data stored independently from the lifecycle of the ECS instance whereas Ephemeral Storage is closely bound to the lifecycle of your ECS instance. That is, data stored on persistent storage won't get lost in cases of ECS restarts or hot migration, or when you delete an ECS instance. It exists independently and can be also mounted easily between different ECS instances. Ephemeral Storage on the other hand will get lost in case of ECS reboots, hot migration (to be more precise it is not guaranteed to be kept) and ECS deletion. Don't use it for long-term storage. Usually, it is a lot faster since this kind of storage is directly attached to the physical host system whereas persistent storage lives in a different dedicated cluster. So every access needs to cross the network. 

For *Persistent Storage* Alibaba Cloud offers the following options:
**Cloud Disk**
Cloud Disk is a high-performance, low latency block storage service for Alibaba Cloud ECS. It supports random or sequential read and write operations. Block Storage is similar to a physical disk. You can format a Block Storage device and create a file system on it to meet the data storage needs of your business. One Cloud Disk can be mounted to *at most* one ECS instance. The ECS instance and the Cloud Disk need to be in the same availability zone. Cross-AZ communication is not supported.

The disks come in three tiers: *Enhanced SSD*, *Standard SSD*, and *Ultra Disk*. *Basic Disks* are no longer for purchase but only exist for backwards compatibility reasons. Each of these tiers share the same maximum capacity of 32.768 GB and data reliability of 99.9999999% (see below paragraph). They differ, however, in maximum IOPS, maximum throughput, and single-channel random write access latency. The maximum IOPS of *Enhanced SSD* is 1 million and the maximum throughput 4000 MB/s with an access latency of around 0,2 ms making it one of the most performant persistent disk options out of every cloud provider. Please consult https://www.alibabacloud.com/help/doc-detail/25382.htm for details.

Each piece of data is also replicated three times across the block storage cluster in the same availability zone to ensure a data reliability of 99.9999999% during read and write operations. Thus, any extra redundancy mechanism such as RAID 1 is not recommended.

Cloud Disk also supports encryption based on the industry standard AES-256 and directly integrates with Alibaba Cloud Key Management Service (KMS). Both system and data disks are supported. The key management infrastructure of Alibaba Cloud conforms to the recommendations in (NIST) 800-57 and uses cryptographic algorithms that comply with the (FIPS) 140-2 standard. When encryption is enabled every snapshot you take is also encrypted. Please note that currently, key rotation is not natively supported.

**Shared Cloud Disk**
Shared Block Storage is a block-level data storage service that features high concurrency, high performance, and high reliability. It supports concurrent reads and writes on multiple ECS instances, and provides data reliability of up to 99.9999999%.
In a traditional cluster architecture, multiple computing nodes access the same copy of data to provide services. To prevent service disruptions due to single point of failures, you can use Shared Block Storage to ensure access to the data, achieving high availability. We recommend that you store business data in Shared Block Storage devices and use a cluster file system such as Google File System (GFS) and General Parallel File System (GPFS) to manage these devices. Data consistency can be guaranteed between multiple front-end computing nodes during concurrent read/write operations.

Note that a single Shared Block Storage device can be attached to a maximum of eight ECS instances in the same zone and region at the same time. Cross-AZ Shared Cloud Disks are not supported.

**Network Attached Storage (NAS)**
NAS is a distributed file system that features shared access, scalability, high availability, and high performance. Based on POSIX file APIs, NAS is compatible with native operating systems. This ensures data consistency and exclusive locks during shared access. It provides data reliability of 99.999999999% (eleven nines).

NAS supports standard protocols, such as NFS V3.0 and NFS v4.0. and SMB 2.1 and later versions, with corresponding support for Windows 7, Windows Server 2008 R2 and all later versions of Windows, but does not support Windows Vista, Windows Server 2008 and earlier versions. NAS provides data consistency and file locking based on POSIX file APIs.

NAS comes in three different service-tiers: *NAS Capacity*, *NAS Performance*, and *NAS Extreme*. They are suited for different workloads and differ in terms of IOPS, latency, and throughput. While *NAS Capacity* supports volume sizes as big as 10PB with a relatively high latency of 10ms, *NAS Performance* optimizes for throughput and latency but has a reduced volume size of 1 PB at max. See https://www.alibabacloud.com/help/doc-detail/61136.htm for details.


For *Ephemeral Storage* Alibaba Cloud offers **Local Disks**.

Local disks are disks that are attached to the same physical machine that hosts their ECS instance. Local disks provide local storage and access for ECS instances. Local disks are cost-effective and provide high random IOPS, high throughput, and low latency. As discussed previously it is not suited for long-term storage since in case of reboots or hardware failures data may get lost. Data redundancy must be implemented at the application layer by yourself, as well as any encryption.

Note that local disks are only available with certain instance families. These include `i2, i2g, i2ne, i2gne, gn5`, and `ga1`. Please consult https://www.alibabacloud.com/help/doc-detail/63138.htm for a detailed overview of the performance characteristics.

### OSS
The storage options in the previous section where all members of the so-called block-level storage which supports random read/write patterns making it suitable for any kind of computing where you need fast and efficient access. Alibaba Cloud Object Storage Service (OSS) in contrast is a so-called Object Storage. Files are not split into evenly sized blocks of data but organized in a flat object hierarchy that is composed of the content, meta data, and a globally unique identifier. Random read / write patterns are not supported. It scales very well, though, is much cheaper than block-level storage and particulary suited to store large amounts (PB-level) amounts of data for batch analyses in the area of AI and Big Data.

Data is organized in so called buckets which is a FQDN and globally unique. A bucket has unlimited capacity. A single object can be at most 48,8 TB in size.
OSS provides strong consistency per default. That is after you have created or updated an object every read operation will *always* get the most recent version.

It also supports asynchronous cross-region replication. It uses our own internal network. So data is not replicated across the public internet. There is no dedicated bandwidth, however. If you need dedicated bandwidth and predictable data copy duration we recommend to use Cloud Enterprise Network (https://www.alibabacloud.com/help/product/59006.htm) which lets you configure a dedicated bandwidth between 2 Mbits and 10Gbits. 

For migration scenarios where you need to copy over data from other Object Storage vendors such as AWS S3, or Azure Blob Storage, Alibaba Cloud offers the fully managed service *Data Online Migration* (https://www.alibabacloud.com/help/product/94157.htm). It supports a wide array of different third-party object storage services including self-hosted solutions that offer a public HTTPS endpoint.


## Network Performance 
Let's quickly define *outbound* and *inbound* traffic:
- Inbound refers to network traffic that is sent from the public internet to any Alibaba Cloud service (i.e. traffic flows *into* the cloud)
- Outbound refers to network traffic that is sent from any Alibaba Cloud service to the public internet (i.e. traffic *leaving* the cloud)
### ECS - External Performance
Inbound traffic is at *minimum* 100MBits. It will be *at most* as high as EIP Bandwidth. 

Outbound traffic is capped by the EIP bandwidth. Bandwidths greater or equal 1Gbits can only be saturated by multiple threads.

Note that the maximum default EIP bandwidth is 200 Mbits, the maximum instance-bound public IP bandwidth is 100 Mbits.

In order to increase that you have to add your EIPs (no instance-bound public IPs are supported) to a shared internet bandwidth package which can be as high a 1Gbits (see https://www.alibabacloud.com/help/doc-detail/55784.htm for details). There are not additional costs for a shared bandwidth internet package. This way you can increase you outbound bandwidth to up to 1 Gbits. This bandwidth can only be saturated by multiple threads, though. 
You can also create multiple bandwidth packages of course.

To further increase the external network performance you can also use mutliple ENIs (Elastic Network Interfaces) and bind up to 10 EIPs to up to 10 private ip addresses of a single ENI in NAT mode. By assigning multiple bandwidth packages to these EIPs you can further increase the network throughput of a single instance. Please check https://www.alibabacloud.com/help/doc-detail/88991.htm for further details on ENI and the different supported modes such as *Cut-Through mode* and *Multi-EIP to ENI mode*.

### ECS - Internal Performance
Both inbound and inbound bandwidth limits of an ECS instance are usually the same and do not differ. They depend on the instance family and type.

For regular instance types they vary between 0.5 Gbits and 25 Gbits.

The new generation of G6 instance types also provide burstable bandwidth capacity which allows the bandwidth to go up as three times as high as the base bandwidth for a short amount of time. See https://www.alibabacloud.com/help/doc-detail/108490.htm for details on the exact numbers. 

Super Compute Cluster (SCC) types instances can achieve 2x25 Gbits via RDMA over Converged Ethernet (RoCE). 

### Service Loadbalancer (SLB) Network Performance
The outbound (SLB to Internet) network performance depends on the region the SLB is created. It ranges from 1Gbit/s up to 5 Gbit/s. You can get a detailed breakdown by region at https://www.alibabacloud.com/help/doc-detail/85966.htm

For intranet network performance the limit is always 5 Gbit/s, independently from the region the SLB is running in.

### Object Storage Service (OSS)
There are two performance limits which need to be taken into consideration when designing your system based on OSS. The capacities described below are reserved at account level, meaning they are shared between your individual buckets.

- **Bandwidth**: Per default, 10 Gbit/s are reserved for both inbound and outbound in Mainland China regions, whereas 5 Gibt/s are reserved outside of Mainland China regions. These limits are soft-limits. That is they can be increased by opening an according ticket. For the public endpoint the bandwidth is mainly limited by the local bandwidth of the client and the quality of the network provided by operators. So if possible it is recommended to use the internal endpoint if possible.
- **Operations**: OSS can sustain 2000 operations per second per partition (downloading, uploading, deleting, copying, and obtaining metadata are each counted as one operation, while deleting or enumerating more than one files in batch is considered as multiple operations). OSS automatically and constantly partitions your data into up to 65,536 partitions based on the prefix of your filename. So make sure to follow according best-practices outlined at https://www.alibabacloud.com/help/doc-detail/64945.htm when defining a naming schema for your object names.

So if you are experiencing performance bottlenecks make sure to look at both aspects when troubleshooting your system.