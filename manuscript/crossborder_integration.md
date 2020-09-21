{id: ch-cross-border}
# Global Cross-Border Integration
In this chapter we will explore the various networking services Alibaba Cloud offers to implement and manage global and hybrid cross-border network integration projects, and also looks briefly at the practical implications of the Cyber Security Law and ICP licensing.
When we say *global cross-border networks*, we are referring to a network that spans at least two international geographies, countries, or cloud regions, including Mainland China. Such a network can either be entirely cloud-based, or it can also encompass external networks from third-party vendors or on-premises networks. In the latter case we call it a hybrid (cross-border) network.
Let's look at each of these network setups in more detail and conclude this section with an overview and use-case for Alibaba Cloud Global Accelerator which commplements the network service portfolio by enabling accelerated public endpoints world-wide via Alibaba Cloud's own private backbone network.

## VPC-Peering with Cloud Enterprise Network
Cloud Enterprise Network (CEN) allows you to peer up to 20 arbitrary Virtual Private Cloud (VPC) networks per region on Alibaba Cloud with each other. So theoretically you can peer up to 20 x *number of cloud regions* with one another per CEN instance. Routing rules can be defined on a fine-granular basis with so-called routing maps. 

The pricing model works on a subscription basis where you buy one or multiple bandwidth packages with a pre-defined full-duplex (send and receive at full bandwidth) bandwith anywhere between 2Mbps and 10Gbps. Depending on the geographies you would like to connect you need different bandwidth packages. For example, in order to peer a VPC in our Frankfurt region with a VPC in our Shanghai region, you need to buy a bandwidth package *Mainland China - Europe*. Any combination (except *Australia - Australia* since it only has one cloud region) of the following geographies are available and only differ in the price per Mbit.
- Mainland China
- Europe
- North America
- Asia Pacific
- Australia

Especially for connections to Mainland China we usually see latencies of no more than 150ms and almost zeor packet loss.

CEN can also be used for intra-region (i.e. VPCs are all in the same cloud region) peering where no bandwidth package is needed and hence is for free as traffic is not charged separately. 

An interesting feature of CEN bandwidth packages are that they can be scaled up and down dynamically anytime to account for changing bandwidth requirements. This can also be automated based on time or different network metrics such as the average usage of bandwidth over a certain timespan. Please refer to the open-source CEN-Scaler at https://github.com/arafato/CEN-Scaler which enables to automatically scale Cloud Enterprise Network (CEN) bandwidth packages based on different metrics and timing events. It ships together with Alibaba Cloud Function code and Terraform templates that set up all neccessary configurations and services to get you going fast.

Keep in mind, however, that downscaling a bandwidth packet during a subscription period is only supported in INTL Portal. It is not supported in Domestic Portal (only upscale).
    
### Quick Facts Bandwidth Scaling
- The change of the bandwidth is effective immediately. Meaning, the updated bandwidth can be used immediately by your applications, and it takes immediate effect on your bill.
- The billing granularity is in seconds. The length of the billing period depends on the subscription type:
    - For monthly subscription it is always exactly 30 days independent from the calendarian length of a particular month.
    - For a yearly subscription it is always 12 months à 30 days
- The effective billing in both up- and downscale scenarios is always based on the remaining time of the billing period, and will result in a dedicated item on the bill. So, in case of a monthly subscription this is the time in seconds until the end of the month, in case of a yearly subscription this is the time in seconds until the end of the year.
- Be aware that an upscale might result in very large billing items since the price is calculated upfront until end of the month (monthly subscription) or even until end of year (yearly subscription). Thus, make sure that the customer has a large enough credit limit on his Alibaba Cloud account, even though he might never actually spend it.
- Every up- and downscale action results in an additional item on the customer’s bill. In case of an upscale it is a billing item, in case of a downscale it is a refund. In below picture you can see an according excerpt from a real billing where the customer scaled up the bandwidth for roughly 2 days and then downscaled the bandwidth again.

See https://www.alibabacloud.com/help/doc-detail/130927.htm for details and please consult the ![CEN-Pricing Document](09/cen_price_doc.pdf) that provides in-depth examples of CEN bandwidth pricing calculations.

## Hybrid Networks
Alibaba Cloud provides and supports multiple ways to connect your on-premises or any other cloud-vendor network to Alibaba Cloud:
- Express Connect
Alibaba Cloud Express Connect helps you build internal network communication channels that feature enhanced cross-network communication speed, quality, and security. Express Connect also helps you mitigate network instability and data breaches. It allows you to connect a leased line to any of the Alibaba Cloud access points. We recommend to contact any of our official network service providers NSP who will help you to establish one or more physical connections and connect your on-premises data center to an Alibaba Cloud VPC. A full list of NSPs access points can be found at https://www.alibabacloud.com/help/doc-detail/96019.htm 
- VPN Gateway
Alibaba Cloud VPN Gateway is an Internet-based service that securely and reliably connects enterprise data centers, office networks, or Internet-facing terminals to Alibaba Cloud Virtual Private Cloud (VPC) networks through encrypted connections. VPN Gateway supports both IPsec-VPN connection and SSL-VPN connection. You must configure the Maximum Transmission Unit (MTU) limit of the local VPN Gateway (that is your premises) to not more than 1,400 bytes. We recommend that you set the MTU to 1,400 bytes. 
- Smart Access Gateway
Smart Access Gateway (SAG) is a software-defined wide area network (SD-WAN) solution developed by Alibaba Cloud based on cloud-native technologies. SAG provides a more intelligent, reliable, and secure approach for enterprises to migrate their workloads to Alibaba Cloud.
It comes with different available configurations of hardware devices and allows for zero touch provisioning and native integration into Alibaba Cloud.
- Equinix Platform and Equinix ECX Fabric which enables an easy and private connection to Alibaba Cloud and supports among other locations
Frankfurt, Dubai, Hongkong, Jakarta, London, Singapur, Sydney, Tokio, Chicago, Dallas und Denver. More information can be found here: https://www.equinix.com/platform-equinix/

All of these solutions for last-mile connectivity can be used in combination with CEN which allows you to connect on-premises networks with each other that may be in Mainland China and overseas regions in a reliable and performant way. While the last mile is for example IPSec, the rest is being transmitted over Alibaba Cloud's private backbone network. This allows for fully automatable and and quick cross-border network integrations world-wide.

## Global Accelerator
Global Accelerator (GA) can provide access points worldwide and accelerate transmission of public network traffic. The GA service guarantees high-quality Border Gateway Protocol (BGP) bandwidth and high service reliability and helps businesses accelerate global connections to Internet-facing services. Backed by the reliable and congestion-free global network of Alibaba Cloud, GA provides high-speed networking experience and ultra-low transmission latency for users across regions.
GA assigns an accelerated IP address to each acceleration region in an acceleration area. Clients from an acceleration region can connect to the access point nearest to the clients through the accelerated IP address. The access point receives client requests and forwards the requests over the Alibaba Cloud global network. GA then automatically selects routes and distributes client requests to the optimal endpoints to avoid network congestion and reduce network latency. Endpoints can be IP addresses or domain names of origin servers.

Let's quickly discuss the bandwidth package options in more detail. Below figure gives an overview about the current options. For accelerating areas and/or endpoints to and from Mainland China, Alibaba Cloud provides two options:
1) *Cross-Border Plan + Standard or Enhanced Plan*. By choosing this option traffic is routed over Alibaba Cloud's private backbone network once the request arrives at the nearest access point. Access points for accelerated regions are located in Mainland China. Thus companies using this configuration need a valid ICP filing or license.
2) *Premium Plan*. With this configuration the access points are located in Hong Kong which does not require an ICP filing or license. Traffic to and from Mainland China is routed via the public CN2 network operated by China Telecom with only minimal additional latency while still offering exceptional and reliable network connection quality. This is especially useful in scenarios where companies need more time to get an ICP license but already want to deliver their digital services or websites to Mainland China via an optimized network connection. Still, we recommend this as a temporary solution. For serious business intentions we recommend to acquire an ICP license.   

![Global Accelerator - Bandwidth Packages](09/ga_bwpackages.png) 

## Solution: Private Global Internet Accelerator
This chapter describes how to accelerate http(s)-requests to any SaaS application and public service from workloads running on Alibaba Cloud by using Cloud Enterprise Network and a redundant setup of an nginx-based reverse proxy that will also do DNS resolution locally to mitigate any DNS spoofing attacks.

### Introduction
From local startups to Small-Medium Enterprises (SMEs) to Multi-National Companies (MNCs). Running and operating globally distributed IT-landscapes, -processes and -workflows for the most business-critical applications has become the new normal in our globalized economy. With employees and customers distributed all over the world many have realized the need to provide global communication and service platforms that are not operated in local silos but rather in a global and coherent way that maximizes synergies and cost-efficiency. 
The bandwidth of scenarios is very broad and diverse. It starts with pretty basic things such as easy-to-use fileshares between employees located in Europe and China, smooth online video calls and meetings between multiple countries such as Munich, Shanghai, and Sidney, and finally ends at very complex scenarios such as globally interconnected SAP workloads and globally distributed IoT-Platforms that manage and analyse millions of devices in different countries and legislations in real-time.

### Challenges
Maintaining a reliable Quality of Service (QoS) for globally distributed workloads that need to interact with each other to support business-critical workflows and processes is challenging, however, due to the following reasons:
-	Reliability: Public internet bandwidth of many regions and countries such as Mainland China is limited and thus not reliable resulting in high packet loss and high roundtrip latency.  
-	Flexibility: While multi-national telco providers provide private, reliable and high-bandwidth offers (e.g. leased lines) they are usually not very flexible. They require commercial long-terms commitments of multiple months or even years, are hard to change configuration-wise, do not support automation and modern DevOps practices very well, and also do not scale easily with changing bandwidth requirements.
-	Security: To decrease the attack surface the entire networking traffic should not be routed through the public internet until it reaches its local internet breakout.
-	Compliance: Global workloads need to adhere to the local law and regulations of the respective countries they operate in. In multi-national setups it is crucial to account for the individual compliance laws and regulations such as the GDPR in Europe and the Cyber Security Law in Mainland China. In particular, it is important that any of such regulations are also met by the network connectivity providers.

Every point can be easily a blocker or at least a reason to reduce speed for any project that includes multi-national interconnected IT-workloads. 

### Scenario
In the remainder of this article we would like to focus on a running example that we often find with many of our customers. As depicted in figure 1, workloads deployed on Alibaba Cloud need to communicate with external public services that are located in a different geographic region of the world. In our example, an application in the German region of Alibaba Cloud needs to communicate with an external public server (or service) which is located in Beijing. The other way round is of course also possible which is depicted at the lower side of this picture where an application deployed in Shanghai region needs to communicate with a public service in the United Kingdom (UK).

![Example Scenario - Cross-Border Access to External Public Services](09/scenario.png)

While this seems not utterly complex and easy to implement at first glance some important considerations have to be taken into account:
-	Public internet bandwidth to Mainland China is limited and not reliable often resulting in packet loss and high latency
-	Dedicated private lines and bandwidth are expensive and are usually hard to easily scale up and down with your demands
-	Networking operators, services and data design need to adhere the respective local legislations and laws such as GDRP, Cyber Security Law (CSL) and MPLS 2.0

### Solution Design
Alibaba Cloud makes it simple to address these challenges and to quickly setup an environment where our applications and workloads can reliably communicate with these public services by building on top of Alibaba Cloud’s world-class and battle-proven networking services. At the same time Alibaba Cloud ensures that its cloud platform meets the MLPS 2.0 baseline and GDPR.

The three major building blocks will be Alibaba Cloud Enterprise Network (CEN), a DNS Private Zone configuration, and a redundant pair of reverse proxies (nginx) which are run on our Elastic Compute Service (ECS) across two availability zones and exposed to the application over an internal load balancer (SLB). Throughout the remainder of this section we will focus on only one networking direction for the sake of simplicity. The other networking direction can be setup analogously.

![Solution architecture for a private global internet accelerator](09/solution.png)

The solution works by defining the domain names you like to have DNS-resolved and forwarded by the proxy in a Private Zone and have them pointed to the proxy or internal SLB IP address respectively. In our example this would be *.mydomain.com (be sure to turn off recursive proxy resolution in PrivateZone if you are using wildcards). Every other domain will be resolved locally and be forwarded. So any http-request against such a domain will be forwarded to the proxy. DNS resolution and subsequent http-forwarding will then be handled by nginx. We will look at the required configuration files for nginx later in this article.

Let’s briefly look at Cloud Enterprise Network (CEN). It is a highly-available network built on the high-performance and low-latency global private network provided by Alibaba Cloud. By using CEN, you can establish private network connections between Virtual Private Cloud (VPC) networks in different regions, or between VPC networks and on-premises data centers which is routed over Alibaba Cloud’s private backbone network. CEN supports automatic route distribution and learning, which speeds up network convergence, improves the quality and security of cross-network communications, and interconnects all network resources. The Alibaba Cloud transmission network is optimized and maintained to ensure that data can be transmitted across regions with a 99th percentile (P99) of per-hour packet loss lower than 0.0001%. 
Bandwidth of CEN can be scaled up and down anytime from 2Mbps up to 10Gbps and will be charged on a second-granularity. It can also be automated based on different metrics via scripts. For a detailed discussion and solution on this please refer to https://github.com/arafato/CEN-Scaler
Once we attach both VPCs to the CEN instance traffic is automatically routed between these two VPCs. Be sure to not use overlapping IP-ranges, otherwise there might be routing issues and errors.

The second building block will be NGINX (https://www.nginx.com/) which will act as our reverse proxy. It will proxy any request from the client application in Frankfurt region to the destination which also includes DNS requests. We will set up a redundant pair of NGINX servers across two availability zones and distribute traffic between the two using a Service Load Balancer (SLB) in front of them.

Before we look at detail at the configuration let’s think about the necessary configuration of the ECS instances in more detail with a specific focus on the Outbound and Inbound internet bandwidth which is important for our scenario. Let's quickly define inbound and outbound traffic:
-	Inbound refers to network traffic that is sent from the public internet to any Alibaba Cloud service (i.e. traffic flows into the cloud)
-	Outbound refers to network traffic that is sent from any Alibaba Cloud service to the public internet (i.e. traffic leaving the cloud)

Inbound traffic is at minimum 100MBits. It will be at most as high as EIP Bandwidth. 
Outbound traffic is capped by the EIP bandwidth. Bandwidths greater or equal 1Gbits can only be saturated by multiple threads. Note that the maximum default EIP bandwidth is 200 Mbits, the maximum instance-bound public IP bandwidth is 100 Mbits.

In order to increase that you have to add your EIPs (no instance-bound public IPs are supported) to a shared internet bandwidth package which can be as high a 1Gbits (see https://www.alibabacloud.com/help/doc-detail/55784.htm for details). There are no additional costs for a shared bandwidth internet package. This way you can increase you outbound bandwidth to up to 1 Gbits. This bandwidth can only be saturated by multiple threads, though. You can also create multiple bandwidth packages of course.

To further increase the external network performance you can also use mutliple ENIs (Elastic Network Interfaces) and bind up to 10 EIPs to up to 10 private ip addresses of a single ENI in NAT mode. By assigning multiple bandwidth packages to these EIPs you can further increase the network throughput of a single instance. Please check https://www.alibabacloud.com/help/doc-detail/88991.htm for further details on ENI and the different supported modes such as Cut-Through mode and Multi-EIP to ENI mode.

Let’s go through step by step through the configuration:

1. Install and configure nginx on both ECS instances
Depending on your Linux distribution you are using commands may vary. Please consult your distribution documentation on how to install nginx.

After it has been installed add the following configuration to `/etc/nginx/nginx.conf`
Make sure to make a backup of the file prior to the modification.

```
# Configure stream forwarding of https protocol stream {
  map $ssl_preread_server_name $backend_pool {
      # Explicitly configure allowed domain names
      ~.*\.mydomain\.com $ssl_preread_server_name:$server_port; 
     default "";

  }

  server {
    listen 443;
    ssl_preread on;
    resolver 8.8.8.8;
    proxy_pass $backend_pool;
  }
}
```

This will allow to resolve and forward the explicitly specified domain names (e.g. *.mydomain.com) to be resolved by any DNS name server you may want to use. In our case, it is the Google DNS name servers which is available under 8.8.8.8, but could be any other DNS name servers which are reachable by the proxy of course.
Often you also need more control over the HTTP headers and possibly error codes you would like to return as a result of any http-call to your proxy. In order to do this you can modify the server.location block in your nginx configuration-file as follows:

```
server {
…
    location / {
     	set $is_allowed 0;
if ($host ~ '.*\.mydomain\.com') {
            set $is_allowed 1;
}
if ($is_allowed = 0) {
            return 404;
}

      proxy_set_header Host $host;
      proxy_set_header Accept-Encoding "";
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Cookie $http_cookie;

      resolver 8.8.8.8;
      proxy_pass http://$host:$server_port$request_uri;
    }
}
```

You can modify this more specific to your requirements, of course. 
Also do the same configuration on the second ECS instance. After that restart the nginx-server to make the new configuration effective. Depending on your distribution this can be done for example via 
`$ systemctl restart nginx` 

2. Load Distribution with SLB
In order to distribute requests equally on both proxy machines you also need to setup an internal SLB in the same region as the proxy machines. It will not be exposed to the public internet. Then add the two ECS instances with the nginx proxy running to the standard server group and create a TCP listener that forwards any request to the proxies on ports 80 and 443. For DNS requests we recommend to use a UDP listener which is listening and forwarding on port 53.
The web-based wizard will guide you through the entire process. If not sure which values to choose just go with the default values for now and adapt later for specific requirements.

3. Private Zone
Now it is time to configure the Private Zone which is part of the Alibaba Cloud DNS service.
![Solution architecture for a private global internet accelerator](09/private_zone.png)
Simply add the zones you want to forward, add according entries that point to the internal proxy IP (or in case of a redundant setup to the internal IP of the SLB). Then bind the VPC where the client machines are deployed to this Private Zone. The configuration is then effective immediately.

4. Security Groups Configuration
Once this has been done, there is one last thing left to configure. You have to grant the client machines explicitly access to the proxy machines. You do that by defining a so-called security group which is associated to the proxy machines.

Obviously, this needs to be adapted with your specific configuration. But the point is, you need to explicitly grant the client machines (aka “Authorization Object”) access to the proxy machines over all required ports which are usually 80, 443 for http(s) and 53 for DNS.

If all of this set up you can check if the setup is working by doing a ping-request against a public endpoint like so

`$ ping my.endpoint.com`

Which should resolve into the internal IP-address of the proxy or SLB IP.
You can then send requests over the proxy to the according public endpoints of yours.

### Conclusion
In this section we looked at how to build a private internet accelerator based on Alibaba Cloud Enterprise Network service and nginx to accelerate DNS and http(s) requests over Alibaba Cloud’s private high-performance, low-latency network to external endpoints. 
We discussed how to setup each one of these components, what to keep in mind when selecting the according ECS instances in terms of networking bandwidth and what kind of challenges can be easily addressed, both from a technical point of view and a compliance point of view.


## ICP License Quick Facts
### When is an ICP-License needed?
An ICP Filing License is needed if
-	You host a server or running a service in mainland Chinese that has a public IP and distributes web-content. The origin of the data does not matter.
-	You are using a domain name (e.g. mydomain.com) associated to an IP address or a CDN network that is located in mainland China

An ICP Commercial License is needed if
-	the requirements of ICP Filing are true
-	if the onshore website is a “for profit” website, meaning financial transactions are being made. 

By web content we mean any human- and machine-readable data that is publicly served on port 80 and 443. Using other ports do not release you from obtaining an ICP license, but may delay any blocking from MIIT. This is not recommended, though, due to below reason.

Keep in mind, though, that any ICP-violation will result in having your IP and domain name blacklisted, effectively blocking your website from any further access. It is very hard and time-consuming to have it removed from this blacklist ever again.

Application Duration: 
-	ICP Filing: 2-4 weeks
-	ICP Commercial: 4-8 weeks

### Are there any requirements a domain name has to satisfy?
Yes, there are two main requirements:
-	The domain needs to be registered at a Chinese registrar. Domains registered at our INTL site are not considered a Chinese registrar and thus not eligible for ICP license! Either guide the customer to a third-party Chinese registrar such as GoDaddy (or any other registrar verified by CNNIC (China Internet Network Information Center) (https://cnnic.com.cn/syjszc1/List/201210/t20121011_36680.htm), or refer to our Domestic Portal which is also verified by CNNIC.

-	Only the following Top-Level Domains are eligible for ICP License: 
    - .com
    - .net
    - .org
    - .asia
    - .cn

Additionally, the domain name must not include sensitive words that might conflict with local Chinese law.
Can I apply for an ICP license without a Chinese business entity?
Theoretically, yes. This can be done through so called ICP-Proxy Providers. This falls into a regulatory grey area so do not recommend this to your customers unless you know exactly what you are doing and understand the following risks:
-	You need to sign over your domain name to the ICP-Proxy provider meaning you are losing your claims under public law on this domain
-	If the original owner of the ICP-License violates Chinese law, all domains and IPs associated with this ICP License will be put on the blacklist

### Serving Content from outside China
Can I serve web-content to Chinese customers from servers outside of Mainland China and thus avoiding the need for an ICP-License? Generally speaking, yes. By doing this over public internet you will usually suffer high latency and packet loss, and very unreliable connections. Your website may also be blocked temporarily (e.g. maybe you are running your website on a shared IP-address which gets blocked for reasons out of your control).

**Alibaba Cloud Global Accelerator Premium Bandwidth Plan**

This offering uses Global Accelerator with public IPs located in HongKong. The traffic from Mainland China to HongKong is routed via via the public CN2 network of China Telecom. From there the private backbone network of Alibaba Cloud is being used. As an implication, no ICP Filing is needed since IPs are not located in Mainland China. This is useful if you need to start as quickly as possible with a project and cannot wait for an ICP license or your legal business entity in Mainland China, respectively. We recommend, however, for any serious production services to acquire an ICP license later.

**Alibaba Cloud Anti DDoS-Premium “Mainland China Acceleration”**

This offering creates a public IP hosted in HongKong (which can connect to your website no matter if it is hosted on Alicloud or somewhere else). The connection is then routed to Mainland China internet through China Telecoms CN2 network (https://www.cteurope.net/products-services/internet/global-ip-transit/) 
Specifics:
-	Website can be hosted anywhere (as long as it has a public endpoint)
-	Still in BETA, needs to be whitelisted in your account. Contact chenfeng.zp@alibaba-inc.com 
-	Built-in Anti-DDoS protection
Price-Points:
-	Comparatively expensive since you need to buy an Anti-DDoS Premium Subscription which starts at around USD 2600 / month

## Who can (and cannot) apply for an ICP-License?
-	Chinese-owned businesses with a Chinese business license can apply for a Business ICP (企业备案)
-	Partially or wholly foreign-owned (non-Chinese) businesses with any type of Chinese business license (Joint-Venture or WOFE, for example), can apply for a Business ICP
-	Chinese nationals, using their state-issued ID, can apply for an Individual ICP (个人备案)
-	Foreign (non-Chinese) individuals, using their passport as ID, who can be physically present in China long enough to fulfil some basic registration requirements, can apply for an Individual ICP

The following entities may not apply for an ICP:
-	Foreign businesses with no legal business presence in China
-	Foreign individuals without a passport (and who are therefore ideally not residing in China)

## What does the application process look like?
1)	You submit your ICP application form and documents to Alibaba Cloud. We check them and submit them to the provincial government branch of the Ministry of Industry and Information Technology (MIIT - gong ye he xin xi hua bu - 工业和信息化部), the Chinese government agency responsible for issuing ICP licenses. 

This process is fully supported by our web portal at https://beian.aliyun.com/order/selfBaIndex.htm 
Since it is in Chinese, our customers may need support from a Chinese speaking employee.

2)	If the application is approved, MIIT notifies Alibaba Cloud, and then we unlock the customer’s account. The customer will never directly interact with MIIT, this is done by Alibaba Cloud.

3)	After you have received your ICP-Filing number you need to put it on your website’s footer and link to http://www.miitbeian.gov.cn 

4)	Last but not least you need to finally apply for a PSB-Filing and also enter the according information on your website. The process is described here:
https://www.alibabacloud.com/help/doc-detail/43898.htm

For deeply detailed information please refer to https://www.alibabacloud.com/help/product/35468.htm
For a graphical overview of the entire process please refer to below graphic (courtesy of Kendra Schaefer, Web Design Tuts+):
![ICP-License Application Process Outline ](08/icp-process.png)
