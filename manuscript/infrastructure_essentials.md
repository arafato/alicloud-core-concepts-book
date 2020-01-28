# Infrastructure Essentials
This chapter gives a focused and very condensed rundown on the very essentials on the Alibaba Cloud Infrastructure service concepts such as Compute, Network, and Storage. Almost like a cheat-sheet.

## Elastic Compute Service (ECS)
The ECS service is organized by so-called *instance families* which in turn consist of different *instance types*.
An instance family describes the fundamental characteristics and use-cases for instances types of this family. Some are optimized for network-intense applications, others are desigend for memory or compute-intense workloads.
As such they usually differ in terms of Core-to-RAM ratio, maximum persistent disk IOPS, network performance (as both in bandwidth and PPS), etc. Please consult the official documentation at https://www.alibabacloud.com/help/doc-detail/25378.htm for detailed numbers.

As of this writing there exist 12 different instance families on Alibaba Cloud:
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

## Storage

### OSS
Cross-region replication over internal network with no dedicated bandwdth, however.

## Network Performance 
Let's quickly define *outbound* and *inbound* traffic:
- Inbound refers to network traffic that is sent from the public internet to any Alibaba Cloud service (i.e. traffic flows *to* the cloud)
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