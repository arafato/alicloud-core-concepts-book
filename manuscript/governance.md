# Governance
Cloud governance is the development and implementation of controls to manage access, budget, and policies for security, compliance, and even high-availability and resiliency across your workloads in the cloud. In this chapter we will focus on the core concepts and according best-practices to implement according controls and policies for most of the before mentioned aspects. 

## Account Management
There are currently two different types (or *memberships*) of Alibaba Cloud accounts: *Individual Account* and *Enterprise Account*. There is no difference in terms of functionality. They differ, however, in terms of real-name verification requirements, free tier offering, and discount eligibility. Let's quickly go through each of them. 
- **Real-Name Verification:** Each account needs to be verified with a real identity if you want to create cloud resources in Mainland China. This is a strict requirement. Resources include OSS buckets and ECS instances for example. Basically, all resources that either expose a public IP address or a public domain name. The verification differs from what kind of identity proof needs to be submitted for each account type. For *Individual Account* you need to submit a proof of your personal identity which could be a copy of your passport. For *Enterprise Account* you need to submit an excerpt from the commercial register. This process is completely supported by the Web Portal and usually takes 3-4 business days to complete.
- **Free-Tier Offering:** Based on the account type the free trial offering differs in terms of how many free credits you get and what kind of products you are eligible for. Please see [https://www.alibabacloud.com/campaign/free-trial](https://www.alibabacloud.com/campaign/free-trial) for details. The *Enterprise Account* has a much bigger free product range and also more free credits.
- **Discount Eligibility:** Throughout the year Alibaba Cloud often has special promotion campaigns which include discounts on certain products. These discounts often differ depending on the cloud account type. Usually, *Enterprise Accounts* are eligible for higher discounts.

If possible, we strongly recommend to choose an *Enterprise Account* since it usually provides higher discounts during promotion campaigns, and also has a stronger free-trial offering. As mentioned before, there is no difference in functionality otherwise.

### Security Settings
Now that we have discussed the two principal account types of Alibaba Cloud, let's look at the core concepts of managing a cloud account. Each cloud account has exactly one and only one root user. You are specifying the login name and password during initial cloud account creation. Each root user also has an associated mobile phone number which can be re-used across different root users at most six times. In section [User and Permission Management](#ch-governance-permission) we will look in detail at how to create additional users and define according permissions.
Below screenshot shows the options of the *Security Settings* administration page which is only available for the root user and accessible directly via [https://account-intl.console.aliyun.com](https://account-intl.console.aliyun.com).

[Account Management - Security Settings](02/am_security_settings.png)   

On this page you can easily change the login password and and your mobile phone number. The mobile number is important as such as it is used as a second authentication factor for changing the password, and also for setting up 2FA for your root user. Last but not least you can also define a login mask that lets you explicitly whitelist IP ranges from which (SSO) login is allowed. Per default, all IP ranges are allowed.

#### Best Practices
1) Enable *Account Protection*, that is 2FA, which currently supports Time-based One-time Password (TOTP) and SMS verification
2) Define a *Login Mask* that only allows login from a specific IP range, e.g. the outbound IP range of your corporate network, thus blocking illegal login attempts from unknown IP ranges.
3) Choose a password that is sufficient in both complexity and length. Consult your security advisor on your companies password guidelines.
4) Never activate *Access Key ID* and *Access Key Secret* for your root user. You can check at [https://usercenter.console.aliyun.com](https://usercenter.console.aliyun.com) whether according keys have been defined. Deactivate them immediately since the root account is not recommended for any programmatic use. Think of it as the root user on Linux systems which has universal access rights to each and everything and whose rights cannot be restricted. For the day to day work the root account should never be used at all!
5) Activate [ActionTrail](https://www.alibabacloud.com/help/product/28802.htm) to fully audit your account. Please see section [Account Auditing](#ch_sec_audit) of chapter [Securing your System](#ch_sec) for details.    

## Notification Management
The so-called *Message Center* which is available at [https://notifications-intl.console.aliyun.com](https://notifications-intl.console.aliyun.com) provides means to get proactively notified about important incidents and updates on and about the Alibaba Cloud platform. These notification messages are divided into five distinct groups:
- **Account Message:** This is about notifications about account expenses. For example, you can subscribe to notifications about overdue bills.
- **Product Message:** This includes updates on product upgrades, configuration changes, price changes, new product launches, but also notifications when services are about to expire and need to be renewed.
- **Fault Message:** Notifications in this group mostly deal with service interruptions, malfunctioning, and system maintenance windows. Really useful to get proactively notified about such kind of unplanned events.  
- **Activity Message:** This is about marketing campaigns and promotions, offline events, and new product and regions launches.
- **Service Message:** This includes notifications on recommended easy-to-use tools, tutorials and lessons-learned to help you use our cloud products even more efficiently.

Each notification can be configured to be delivered either via email or as internal message. While the email receivers can be freely configured, internal messages work just like an email inbox in your Alibaba Cloud web portal which is only accessible by the root user. 

#### Best Practices
While we believe that each notification is valuable for our customers we recommend to activate at least the following ones:
- Account Message - *Notifications of Account Expenses*
- *Product Message* - *Alibaba Cloud DNS High Risk Notification*
- *Product Message* - *Notifications of Product Expiration*
- *Product Message* - *Product Overdue Payment, Suspension, and Imminent Release Notifications*
- *Product Message* - *Notifications of Product Release*
- *Product Message* - *Notifications about product upgrades, configuration changes, and price changes*
- *Product Message* - *New Product Function Launch and Function Removal Notifications*
- *Product Message* - *Security Notice*
- *Fault Message* - **Activate all notifications**

{id: ch-governance-permission}
## User and Permission Management
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Billing Management
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Regions and Availability Zones
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Service Level Agreements
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

