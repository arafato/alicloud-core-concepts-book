{id: ch_sec}
# Securing your system
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Security Center
There is a kernel bug in combination with KVM: https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1858760Our which is exploited by `AliyunDun` (and basically any process that uses loops in combination with process suspension by using usleep() for example.)

Older instance types are affected since they are running on KVM. Our most recent instance types do not run on KVM anymore but on our own hypervisor technology. Thus, they are not affected by this bug.

So there are currently two solutions:
- Either downgrade the Kernel version to <= 4.15.0-70
- Or consistently use the newest generation types of the 6th generation and above (e.g. G6).

## Account Security
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

{id: ch_sec_audit}
## Account Auditing
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Network Security
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

### Anti-DDoS
Alibaba Cloud provides 4 different offerings:
- **Anti-DDoS Basic**
    
    If your workloads are running on Alicloud you'll get a mitigation capacity of 5 Gpbs fo free. Available in any of our 22 cloud regions.

- **Anti-DDoS Pro**
    
    This service covers our 10 regions in Mainland China only. It has a maximum mitigation capacity of currently 600Gbps per instance. It supports workloads on and off Alicloud. The complete mitigation capacity of this offering is around 8 Tbps.

- **Anti-DDoS Premium**
    
    This service covers the rest of our regions outside of Mainland China. It comes with a mitigation capability of 2 Tbps through anycast technology. It supports workloads on and off Alicloud.

The latter two work by always routing the traffic to the closest mitigation center and from there to the origin. This may result in slightly higher latencies. If workloads are latency-sensitive we recommend to use Anti-DDoS Origin which is explained further below.
The price model for the latter two is subscription-based (1 month minimum), that is pre-payed, and is based on the following values:

- Basic Protection Capacity in Mbps

    This is payed upfront. Available configurations are documented at: https://www.alibabacloud.com/help/doc-detail/67901.htm

- Burstable Protection Capacity

    In case you would like to be able to react to bursts you can also choose to buy additional burstable potection capacity. This is charged on a PAYGO basis if there are actual peaks. If there are no peaks higher than your basic protection no additional charges occur.

- Clean Bandwidth Capacity

    This is the clean bandwidth your Anti-DDoS service needs to be able to handle (excluding attack traffic). If your regular bandwidth demands gets higher packet drops may occur.

The fourth offering is called **Anti-DDoS Origin**. It works differently from Anti-DDoS Pro and Premium in that traffic is always routed directly to the origin. Only in case of attacks is the traffic redirected to our global scrubbing centers. This works by announcing according BGP routes to redirect the traffic. For customers outside of Mainland China this is also supported for origins off Alibaba Cloud. For this scenarios customers need to have a BGP network and AS, however. In the event of an attack traffic is then routed either through a GRE tunnel or a cross connect to the origin.