# Infrastructure Essentials
This chapter gives a focused and very condensed rundown on the very essentials on the Alibaba Cloud Infrastructure service concepts such as Compute, Network, and Storage. Almost like a cheat-sheet.

## Elastic Compute Service (ECS)
ECS is the IaaS-service by Alibaba Cloud that provides customers compute power as virtual machines. The security and compliance is a shared responsibility between Alibaba Cloud and the customers.
Alibaba Cloud is responsible for "Security of the Cloud". That is, it is responsible for protecting the infrastructure that runs all of the services offered in the Alibaba Cloud. This infrastructure is composed of the hardware, software, networking, and facilities that run Alibaba Cloud services. For ECS, Alibaba Cloud's responsibility includes everything up to the hypervisor of the host machines that powers the ECS service. Everything above is the customer's responsibility which includes the guest operating system and all security configuration tasks such as configuration of Alibaba Cloud provided firewall (called a security group) on each network interface of an ECS instance. 

### Availability Service Level Agreement (SLA)
ECS comes with a Monthly Single-Instance Availability SLA of 99,95%, and provides a Monthly Multi-Zone Availability SLa of 99,99% if your application is deployed on at least 2 ECS instances spread across two different Availability Zones.

An ECS instance is considered *Unavailable* if the disconnection between an ECS instance configured with access permitted rules and any IP address over TCP or UDP in the inbound and outbound directions lasts for more than one minute.  

### Instance Families and Instance Types
The ECS service is organized by so-called *instance families* which in turn consist of different *instance types*.
An instance family describes the fundamental characteristics and use-cases for instances types of this family. Some are optimized for network-intense applications, others are desigend for memory or compute-intense workloads.
As such they usually differ in terms of Core-to-RAM ratio, maximum persistent disk IOPS, network performance (as both in bandwidth and PPS), etc. Please consult the official documentation at https://www.alibabacloud.com/help/doc-detail/25378.htm for detailed numbers.

Note that ECS configurations and types can be updated, however, not arbitrarily. For instance family and type changes only certain types are supported (see below documentation link). 

As of this writing there exist 13 different instance families on Alibaba Cloud (please consult documentation at https://www.alibabacloud.com/help/doc-detail/108490.htm for details):
- General Purpose
- Compute Optimized
- Memory Optimized
- Big Data
- Local SSDs
- High-Clock Speed
- Compute Optimized with GPU
- Visualization Compute with GPU
- Compute Optimized with FPGA
- Bare Metal
- Super Computing Cluster
- Burstable
- Dedicated Hosts (see next section)

The new generations of Alibaba Cloud x86-based ECS instances are equipped 2.5 GHz Intel ® Xeon ® Platinum 8269CY (Cascade Lake) processors with Turbo Boost up to 3.2 GHz. The newest generation of the General Purpose G family now also support burstable network bandwidth.

For our GPU-based instance types, we currently provide the NVIDIA® Tesla® P4 with the 1/8, 1/4, 1/2, and 1/1 computing capacity support, and the NVIDIA T4 GPU with 16 GB memory capacity (320 GB/s bandwidth),
2,560 CUDA Cores, up to 320 Turing Tensor Cores, and mixed-precision Tensor Cores support for 65 FP16 TFLOPS, 130 INT8 TOPS, and 260 INT4 TOPS. 

Deployment Sets (https://www.alibabacloud.com/help/doc-detail/91258.htm) gives you control on the distribution strategy. You can use a deployment set to distribute your ECS instances to different physical servers to guarantee high availability and set up underlying disaster discovery. When you create ECS instances in a deployment set, Alibaba Cloud will start the instances on different physical servers within the specified region based on your configured deployment strategy. Right now, Alibaba Cloud only provides "High Availability" strategy. As an effect all the ECS instances within your deployment set are strictly distributed across different physical servers within the specified region. The high availability strategy applies to application architectures where several ECS instances need to be isolated from each other. The strategy significantly reduces the chances of services becoming unavailable.

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
- Cloud Disks (both persistent and ephemeral), also focus on shared Cloud Disks, and NAS, and oss-fuse

### OSS
- Cross-region replication over internal network with no dedicated bandwidth, however.
- OSS (also focus on migration scenarios -> ossimport)

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