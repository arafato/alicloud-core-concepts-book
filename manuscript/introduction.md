# Introduction
Technology and in particular today's Public Cloud platforms are evolving fast at an incredible pace while books are rather static with "release cycles" measured in years. Many technology books are already outdated they day they are published. This begs the question why anyone would want to read - let alone write - a book about one of the fastest moving targets in the information technology industry: Alibaba Cloud.

Having spent many years working with different cloud platforms it has become clear that the underlying principles and concepts of just about any cloud platform do not change half as fast as new services and features are being added about almost every single day.
These core concepts serve as the fundament for just about anything that's being built upon and offered by the platform. A thorough understanding and mastery of these concepts is crucial for successfully building and operating anything between a simple web application and the next big thing with millions of users spread around the globe. Many of those concepts are valid for years and are only very carefully touched by the product groups that are building the cloud platform.

This book will dive deep on the core concepts of Alibaba Cloud, and best practices that have stood the test of time and have been successfully applied at many customer project. You will learn how and why things are working the way they are, and how and why you should do things in a certain way when using the Alibaba Cloud platform. Once you got the hang of the underlying principles, and the "Ali-way" of doing things, building, testing, monitoring, and operating will become much more enjoyable. It will also help you reason about the behavior and limitations of our big portfolio of managed services in the area of Big Data, IoT, Analytics, Compute, Storage, and Network.

Over the past years we had the joy of working with many different kinds of customers in different verticals, ranging from small startups, to big enterprise players. As broad and diverse as this range is, as similar are the questions and uncertainties each and everyone of my customers had when they started building and operating real-world workloads on Alibaba Cloud. This book is the result of our accumulated experience we have gained during these years. Most of our Alicloud brain is in it. We hope that you will enjoy reading it and that it will help you in reaching your full potential with cloud-based solutions.

## Why Alibaba Cloud
There are multiple strategic reasons to include Alibaba Cloud into your IT strategy three of which we will detail in the subsequent sections.

### Fast-Paced Innovator that Supports World's Most Demanding E-Commerce Applications.
Alibaba Cloud supports one of the highest demanding online events on earth reliably each year: Double 11, world's biggest shopping festival with a gross merchandized volume of more than 30 billion USD in 24 hours in 2018. To give you a better idea on the dimensions of this event let's drop some numbers on the resources and workloads that are exclusive to this event (2018):
- 10 mio CPU cores provisioned and used
- 325k business transactions per second during peak times
- 1.6 billion network attacks that are successfully mitigated
- 350 million AI-supported support conversations equaling 586k person days
- 1.04 billion orders and deliveries orchestrated, coordinated, and delivered. 

Alibaba Cloud is the technology backbone of a Group and as such constantly pushing the limits of today's technologies to support our own core business. This fast-paced innovation and battle-proven technology is provided to our millions of customers which are often large enterprises by themselves with millions of end-customers. The constant stream of innovation is productized constantly and released frequently as new services and features to build upon and to reliably support business-critical application infrastructure by the millions of our customers world-wide.  

### Second to None Business and Technology Partner for China
Alibaba Group and its cloud division is an international company with Chinese roots. As such we are second to none as a technology and business partner for your IT and innovation projects in South East Asia and China in particular. Alibaba Cloud is the clear market leader with almost 20% market-share in Asia Pacific well ahead any of its cloud competitors such as AWS, Microsoft Azure, or Google Cloud as depicted in below graphic which is the Market Share for IT Services report by Gartner from 2018.  
![](01/market_share.png)
For Mainland China the market-share proportions are even more drastic. Alibaba Cloud took over 43% in the first half of 2018 which is larger than the next seven cloud competitors combined. Forrester Wave also recognizes Alibaba Cloud as the leader in product offerings and market performance in its *Full-stack public cloud developments platforms  in China* report which is depicted below.  
![](01/fw_recognition.png)

With its strong presence in the Asia Pacific region with (as of this writing) 14 regions, 8 of which are in Mainland China, and one additional region in Hong Kong, Alibaba Cloud is the number one choice for global, scalable, and secure application platforms that empower your digital innovation strategy in the APAC regions.
In total, Alibaba Cloud has 20 regions world-wide which also includes North-America (2) and Europe (2) which let's you also benefit from the innovation capabilities of Alibaba Cloud in your local regions while fully complying to the local law and regulation such as GDPR. Below figure gives you an overview about our global footprint of 20 region and 61 availability zones as of now. 
![](01/regions.png)

Note that all of these 20 regions can be managed from one single account. Many services also provide cross-region integration capabilities that let you easily replicate and copy data to and from any of our regions. We will look in depth at the different cross-regional services and features in chapter [Global Cross-Border Integration](#ch-cross-border).

### Geo-Politically balanced multi-cloud strategy
Below graphic shows the Gartner Magic Quadrant for IaaS Worldwide from 2019. If you compare it to the quadrants of previous years you will notice that both the number and geographic heterogeneity has been drastically reduced. In total, there are only six players left, five of which are headquartered in the USA. This leaves Alibaba Cloud as the only hyperscaler which is not under American jurisdiction. A multi-cloud strategy that aims at minimizing geo-political risks and political mutual dependencies should mitigate by implementing a geo-politically balanced multi-cloud strategy. Alibaba Cloud is a reliable and trusted business and technology partner for implementing such a strategy.   
![Magic Quadrant for Cloud Infrastructure as a Service, Worldwide](01/iaas_gartner2019.png)

## Who Should Read This Book
This book is intended for anyone who wants to get a thorough understanding about the core concepts of Alibaba Cloud along with best-practices and migration strategies. If you have a basic knowledge, that will help but it’s not a requirement. We start off with a thorough discussion on the principles of Account Management, Authentication and Authorization, Billing Management, and our geography-based data center architecture. After that we will look at the different concepts and approaches for securing, auditing, and monitoring your accounts and workloads, as well as modern approaches on automating different aspects of your system ranging from service provisioning and configuration to build and deployment pipelines. We will then continue this book with a deep discussion on our best-practices to design for high-available and fault-tolerant systems on Alibaba Cloud. After that we will look at how to integrate geographically disperse applications. Finally, we will devote the last chapter to our recommended approaches and services to design and implement reliable migration paths for different kind of workloads from public cloud providers such as Amazon Web Services.

If you are an architect, a consultant, an administrator, or a technical decision maker who just wants a better knowledge of the Alibaba Cloud core concepts, unique selling points, and best-practices, this book is for you. Many scenarios are also supported by code examples. We do not make any assumptions regarding the reader's level of knowledge.

## How To Read This Book
Here is a glance at what's in each chapter:
- **Chapter 1: Introduction** is where you are right now. 
- **Chapter 2: Governance** focuses on the fundamental aspects of Account Management, User and Permission Management, and Billing Management.
- **Chapter 3: Interacting with Alibaba Cloud** dives into the various means on how programmatically manage and debug our service portfolio. We will discuss the general API model of Alibaba Cloud and tools that support you in your daily work.
- **Chapter 4: Infrastructure Essentials** gives a focused rundown on the very essentials on the Alibaba Cloud Infrastructure services concepts such as Compute, Network, and Storage. 
- **Chapter 5: Securing Your System** discusses proven best-practices and methodologies to secure your account, and networking environment. Additionally, we will also look at how to audit your system to quickly analyze who has done what at which point in time. 
- **Chapter 6: Architecting for High-Availability and Fault-Tolerance on Alibaba Cloud** looks in depth at proven practices and recommendations to make your system and application architecture robust and resilient against sudden unpredictable traffic spikes and service interruptions. It will also look at our geography-based data center architecture, and our Service Level Agreements (SLAs).
- **Chapter 7: Global Cross-Border Integration** explores the various networking services Alibaba Cloud offers to implement and manage global and hybrid cross-border network integration projects, and also looks briefly at the practical implications of the Cyber Security Law and ICP licensing.  
