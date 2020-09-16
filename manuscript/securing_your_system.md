{id: ch_sec}
# Securing your system
System and application security is a very important and complex topic that spans many different aspects and layers of cloud-based applications. From account security to proper rights management with according auditing, to networking security of the entire application and platform services being used, to securing the software that runs on the cloud. A comprehensive discussion would be a book on its own. This is why we will focus on the most important aspects and leave the reader with further reading suggestions to dive deeper into the topics of interest. 

## Security Center
Security Center is a unified security management system that dynamically identifies and analyzes security threats, and generates alerts when threats are detected. It provides ransomware protection, anti-virus protection, web tamper protection, and compliance assessments to ensure the security of cloud resources and local servers. This allows you to automate security operations, responses, and threat tracing, and meet regulatory compliance requirements.

Security Center meets the standards of ISO 9001, ISO 20000, ISO 22301, ISO 27001, ISO 27017, ISO 27018, ISO 29151, ISO 27701, BS1 0012, CSA STAR, and PCI DSS.

Security Center can be used in many ways that help you protecting your cloud environment and your ECS instances in particular. For one, it allows you to whitelist and blacklist certain IP-ranges which you consider eligible to connect to your publicly available ECS instances. In case there are connection attempts (e.g. via SSH or RDP) from IP-ranges you have not explicitly whitelisted you get automatic alarm notifications (e.g. by email) that list in detail from whihc IP and region at what time a connection request was established. All of these events are stored persistently and may also be forwarded to SLS for easy analysis.

Another useful feature is that it automatically assesses what kind of software packages and versions are installed on your ECS machines and whether they are currently affected by any publicly known exploits and bugs. It comes with a one-click feature that lets you patch vulnerabilities instantly and in a coordinated way across fleets of ECS instances.

It also enables you to do automated configuration assessments of your services in use: from your general account protection, to your networking environment, up to the configuration of individual database services. In case risks such as unprotected, public endpoints are discovered it will automatically raise an alarm with suggestions helping you to mitigate the risk appropriately and in a timely manner before attackers can exploit it. Misconfigured databases and applications servers are one of the most often found penetration risks for any IT system. 

An additional feature which is also well integrated into Alibaba Cloud Container Registry service is the Image Security Scanner. 

his service can identify more than 120,000 historical vulnerabilities and detect the latest vulnerabilities. It also provides vulnerability repair solutions to implement one-stop vulnerability management. makes vulnerability fixes easier. It is a One-stop Vulnerability Management covering the entire lifecycle of vulnerability fixes, making vulnerability management easier. It can identify more than 120,000 historical vulnerabilities and also detect the latest vulnerabilities along with suggestions on how to fix them.

Last but not least, Security Center also comes with an anti-virus protection feature which is based on the automated analysis of a massive numbers of virus samples, virus persistence, and attack methods. 

Note that only a subset of these features is available in the Basic edition which is free for all Alibaba Cloud assets. Security Center comes in three tiers: Basic, Advanced, and the Enterprise Edition. The pricing model works on a monthly subscription basis charged by the number of protected assets. It can also be used with any kind of external workloads given that they have access to the public internet to be able to communicate with the public Security Center endpoints. For both internal and external workloads a small agent called 'AliyunDun` needs to be installed on the machines. For detailed installation instructions please refer to https://www.alibabacloud.com/help/doc-detail/68611.htm

Please note that there is a kernel bug in combination with KVM: https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1858760Our which is exploited by `AliyunDun` (and basically any process that uses loops in combination with process suspension by using usleep() for example.)

Older instance types are affected since they are running on KVM. Our most recent instance types do not run on KVM anymore but on our own hypervisor technology. Thus, they are not affected by this bug.

So there are currently two solutions if this bug is affecting your system:
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