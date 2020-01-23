# Infrastructure Essentials
This chapter gives a focused and very condensed rundown on the very essentials on the Alibaba Cloud Infrastructure services concepts such as Compute, Network, and Storage. Almost like a cheat-sheet.

## Elastic Compute Service

## Storage

## Network 
### ECS - External Performance
INBOUND FROM INTERNET is at MINIMUM 100MBits. It will be AT MOST as high as EIP Bandwidth. 

OUTBOUND TO INTERNET is capped by the EIP bandwidth. Bandwidths higher => 1Gbits can usually only be saturated by multiple threads.

As you can see though, the maximum EIP bandwidth is 200 Mbits, the maximum instance-bound public IP bandwidth is 100 Mbits.

In order to increase that you have to add your EIPs (no instance-bound public IPs are supported) to a shared internet bandwidth package which can be as high a 1Gbits. There are not additional costs for a shared bandwidth internet package. This way you can increase you outbound bandwidth to up to 1 Gbits. This can only be saturated by multiple threads, though. 
You can also create multiple bandwidth packages.


### ECS - Internal Performance

### SLB Network Performance
The outbound (SLB to Internet) network performance depends on the region the SLB is created. It ranges from 1Gbit/s up to 5 Gbit/s. You can get a detailed breakdown by region at https://www.alibabacloud.com/help/doc-detail/85966.htm
For intranet network performance the limit is always 5 Gbit/s, independently from the regon the SLB is running in.