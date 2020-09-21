{id: ch-cross-border}
# Global Cross-Border Integration
In this chapter we will explore the various networking services Alibaba Cloud offers to implement and manage global and hybrid cross-border network integration projects, and also looks briefly at the practical implications of the Cyber Security Law and ICP licensing.
When we say *global cross-border networks*, we are referring to a network that spans at least two international geographies, countries, or cloud regions, including Mainland China. Such a network can either be entirely cloud-based, or it can also encompass external networks from third-party vendors or on-premises networks. In the latter case we call it a hybrid (cross-border) network.
Let's look at each of these network setups in more detail and conclude this section with an overview and use-case for Alibaba Cloud Global Accelerator which commplements the network service portfolio by enabling accelerated public endpoints world-wide via Alibaba Cloud's own private backbone network.



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

### Who can (and cannot) apply for an ICP-License?
-	Chinese-owned businesses with a Chinese business license can apply for a Business ICP (企业备案)
-	Partially or wholly foreign-owned (non-Chinese) businesses with any type of Chinese business license (Joint-Venture or WOFE, for example), can apply for a Business ICP
-	Chinese nationals, using their state-issued ID, can apply for an Individual ICP (个人备案)
-	Foreign (non-Chinese) individuals, using their passport as ID, who can be physically present in China long enough to fulfil some basic registration requirements, can apply for an Individual ICP

The following entities may not apply for an ICP:
-	Foreign businesses with no legal business presence in China
-	Foreign individuals without a passport (and who are therefore ideally not residing in China)

### What does the application process look like?
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