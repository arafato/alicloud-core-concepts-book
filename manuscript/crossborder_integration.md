{id: ch-cross-border}
# Global Cross-Border Integration

Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## VPC-Peering with Cloud Enterprise Network    
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Hybrid Networks
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Data Transfer and Management
Hi everybody, very important notice regarding CEN Bandwidth Packages (Mainland China <-> Outside Mainland China):

1) You can upscale any time in both INTL and Domestic Portal
2) Downscaling only works in internation portal, not in Domestic Portal!!

See https://www.alibabacloud.com/help/doc-detail/130927.htm (this doc is only valid for domestic, product group will update it soon)

Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

# ICP License Quick Facts
## When is an ICP-License needed?
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

## Are there any requirements a domain name has to satisfy?
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

## Serving Content from outside China
Can I serve web-content to Chinese customers from servers outside of Mainland China and thus avoiding the need for an ICP-License?
Generally speaking, yes. By doing this over public internet you will usually suffer high latency and packet loss, and very unreliable connections. Your website may also be blocked temporarily (e.g. maybe you are running your website on a shared IP-address which gets blocked for reasons out of your control).

There are currently two offerings to the knowledge of this authors which claim to provide reliable and fast connections to Chinese customers despite being hosted outside of Mainland China:

### WeberCloud (https://www.webercloud-china.de/)
This company is cooperating directly with China Telecom which allows them to have your specific IP-Addresses or domain names requested from mainland China routed through a dedicated tunnel to the data-centre of Weber Cloud in Germany.
Specifics:
-	Website must be hosted in WeberCloud DC
-	Traffic is routed through a dedicated network line
-	Routing is based on IP / Domain name and realized directly by China Telecom
-	Standard bandwidth is 10Mbit/s, but can also be increased
Price-Points
-	Around €150 / month for 10 Mbit/s bandwidth
-	Around €50 / month for website hosting

### Alibaba Cloud Anti DDoS-Premium “Mainland China Acceleration”
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
