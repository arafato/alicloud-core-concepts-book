# Governance
Cloud governance is the development and implementation of controls to manage access, budget, and policies for security, compliance, and even high-availability and resiliency across your workloads in the cloud. In this chapter we will focus on the core concepts and according best-practices to implement according controls and policies for most of the before mentioned aspects. 

## Account Management
There are currently two different types (or *memberships*) of Alibaba Cloud accounts: *Individual Account* and *Enterprise Account*. There is no difference in terms of functionality. They differ, however, in terms of real-name verification requirements, free tier offering, and discount eligibility. Let's quickly go through each of them. 
- **Real-Name Verification:** Each account needs to be verified with a real identity if you want to create cloud resources in Mainland China. This is a strict requirement. Resources include OSS buckets and ECS instances for example. Basically, all resources that either expose a public IP address or a public domain name. The verification differs from what kind of identity proof needs to be submitted for each account type. For *Individual Account* you need to submit a proof of your personal identity which could be a copy of your passport. For *Enterprise Account* you need to submit an excerpt from the commercial register. This process is completely supported by the Web Portal and usually takes 3-4 business days to complete.
- **Free-Tier Offering:** Based on the account type the free trial offering differs in terms of how many free credits you get and what kind of products you are eligible for. Please see [https://www.alibabacloud.com/campaign/free-trial](https://www.alibabacloud.com/campaign/free-trial) for details. The *Enterprise Account* has a much bigger free product range and also more free credits.
- **Discount Eligibility:** Throughout the year Alibaba Cloud often has special promotion campaigns which include discounts on certain products. These discounts often differ depending on the cloud account type. Usually, *Enterprise Accounts* are eligible for higher discounts.

If possible, we strongly recommend to choose an *Enterprise Account* since it usually provides higher discounts during promotion campaigns, and also has a stronger free-trial offering. As mentioned before, there is no difference in functionality otherwise.

### Security Settings
Now that we have discussed the two principal account types of Alibaba Cloud, let's look at the core concepts of managing a cloud account. Each cloud account has exactly one and only one root user. You are specifying the login name and password during initial cloud account creation. Each root user also has an associated mobile phone number which can be re-used across different root users and cloud account respectively at most six times. 

In section [User and Permission Management](#ch-governance-permission) we will look in detail at how to create additional users and define according permissions.
Below screenshot shows the options of the *Security Settings* administration page which is only available for the root user and accessible directly via [https://account-intl.console.aliyun.com](https://account-intl.console.aliyun.com).

![Account Management - Security Settings](02/am_security_settings.png)   

On this page you can easily change the login password and your mobile phone number. The mobile number is important as it is used as a second authentication factor for changing the password, and also for setting up 2FA for your root user. Last but not least you can also define a login mask that lets you explicitly whitelist IP ranges from which (SSO) login is allowed. Per default, all IP ranges are allowed.

#### Best Practices
1) Enable *Account Protection*, that is 2FA, which currently supports Time-based One-time Password (TOTP) and SMS verification
2) Define a *Login Mask* that only allows login from a specific IP range, e.g. the outbound IP range of your corporate network, thus blocking illegal login attempts from unknown IP ranges.
3) Choose a password that is sufficient in both complexity and length, make sure to activate password rotation, and restrict session duration. Consult your security advisor on your company's password and security guidelines. 
4) Never activate *Access Key ID* and *Access Key Secret* for your root user. You can check at [https://usercenter.console.aliyun.com](https://usercenter.console.aliyun.com) whether according keys have been defined. Deactivate them immediately since the root account is not recommended for any programmatic use. Think of it as the root user on Linux systems which has universal access rights to each and everything and whose rights cannot be restricted. For the day to day work the root account should never be used at all!
5) Activate [ActionTrail](https://www.alibabacloud.com/help/product/28802.htm) to fully audit your account. Please see section [Account Auditing](#ch_sec_audit) of chapter [Securing your System](#ch_sec) for details.    

## Notification Management
The so-called *Message Center* which is available at [https://notifications-intl.console.aliyun.com](https://notifications-intl.console.aliyun.com) provides means to get proactively notified about important incidents and updates on and about the Alibaba Cloud platform. These notification messages are divided into five distinct groups:
- **Account Message:** This is about notifications about account expenses. For example, you can subscribe to notifications about overdue bills.
- **Product Message:** This includes updates on product upgrades, configuration changes, price changes, new product launches, but also notifications when services are about to expire and need to be renewed.
- **Fault Message:** Notifications in this group mostly deal with service interruptions, malfunctioning, and system maintenance windows. Really useful to get proactively notified about such kind of unplanned events.  
- **Activity Message:** This is about marketing campaigns and promotions, offline events, and new product and regions launches.
- **Service Message:** This includes notifications on recommended easy-to-use tools, tutorials and lessons-learned to help you use our cloud products even more efficiently.

Each notification can be configured to be delivered either via email or as internal message. While the email receivers can be freely configured, internal messages work just like an email inbox in your Alibaba Cloud web portal which is only accessible by the root user. Below screenshot shows an example of the internal message UI with a notification from the *Product Message* group which announces the general availability of our Serverless Kubernetes offering.

![Message Center Notification](02/am_msg_center.png)

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
Resource Access Management (RAM) is the cloud service which provide means to create additional users (so-called RAM users), and roles with according policies (sometimes also referred to as permissions) that define the access rights on Alibaba Cloud services and specific resources. The interface (i.e. the set of APIs) which is used to manage your cloud resources is usually referred to as OpenAPI which you can interactively explore with the OpenAPI Explorer at [https://api.aliyun.com/](https://api.aliyun.com/). 

Let's break down the different terms we just mentioned and explain what they exactly mean.

### Root User
The initial single sign-in identity that has complete access to all Alibaba Cloud services and resources in the account. This identity is called the Alibaba cloud account root user and is accessed by signing in with the email address and password that you used to create the account. 
Follow the best-practice and use it only to create your first RAM user. Never use it for day to day tasks, and never use it to access your Alibaba cloud resources. 

There are, however, some tasks only the root user can do:
- Modify root user details on the *Security Settings* administration page at [https://account-intl.console.aliyun.com](https://account-intl.console.aliyun.com)
- Delete the Alibaba Cloud account which is accessible via *Security Settings* administration page
- Initial activation of cloud services: most services are deactivated by default and need to be explicitly activated by the root user. This action is irreversible, meaning an activated service can never be deactivated again.    

Most other things (such as whitelisting port 25 for an ECS instance, Reverse DNS entry for ECS) are usually requested by opening an according support ticket which is not restricted to the root user, though.

{id: ch-gov-users-and-policies}
### RAM Users and Policies:
Sometimes also referred to as *Sub-Account*. It is a user account that is used for web-based login to the Alibaba Cloud portal and/or programmatic access to the OpenAPI. They can't access anything in your account until you give them permission. All permissions need to be explicitly granted.
You give permissions to a user by creating an identity-based policy, which is a policy that is attached to the user or a group to which the user belongs. The following example shows a JSON policy that allows the user to perform all TableStore actions (ots:*) on the Books table in the 123456789012 account within the eu-central-1 region.
```
{
  "Version": "1",
  "Statement": {
    "Effect": "Allow",
    "Action": "ots:*",
    "Resource": "acs:ots:eu-central-1:123456789012:table/Books"
  }
}
```
After you attach this policy to your RAM user, the user only has those TableStore permissions. Most users have multiple policies that together represent the permissions for that user. The evaluation policy is as follows:
- By default, all requests are implicitly denied (except for requests by the root user)
- An explicit *Allow* overrides this default
- Any explicit *Deny* overrides any allows. 

There are two access modes you can define: *Console Password Logon* and *Programmatic Access*
The first one is used for web-based login where each actions are being done from the Alibaba Cloud portal. In terms of account protection it follows the same recommended guidelines regarding password security and rotation,  and 2FA. The latter one is meant for being used in combination with our command line interface (CLI) tools such as `aliyun`[^aliyun] or `ossutil`[^ossutil] or with our various SDKs[^sdk]. As such it does not provide 2FA but relies on long-term credentials (Access Key ID and Access Key Secret) to programmatically sign requests to the CLI tools or the OpenAPI.

### Roles
A RAM role is an RAM identity that you can create in your account that has specific permissions. An RAM role is similar to an RAM user, in that it is an Alibaba cloud identity with permission policies that determine what the identity can and cannot do in the cloud account. However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it and is defined as an authorized principal to assume it. Also, a role does not have standard long-term credentials such as a password or access keys associated with it. Instead, when you assume a role, it provides you with temporary security credentials for your role session which consist of an *AccessKeySecret*, and *AccessKeyId*, and a *SecurityToken*. In thi case, the *AccessKeyId* is always prefixed with `STS.`.

Below is an example role definition that allows *every* RAM user (yes, this is a little bit counter-intuitive since we specify `root`as the principal) to assume it who is allowed to invoke `sts:AssumeRole` and whose request originates from within a certain IP range. 
```
{
    "Statement": [
        {
            "Action": "sts:AssumeRole",
            "Effect": "Allow",
            "Principal": {
                "RAM": [
                    "acs:ram::5509671837805201:root"
                ]
            }
            "Condition": {
                "StringEquals": {
                    "acs:SourceIp": [
                        "47.234.42.0/24"
                    ]
                }
            }
        }
    ],
    "Version": "1"
}
```
For example the user can manually invoke `sts:AssumeRole` with the CLI just like that:

`aliyun sts AssumeRole --RoleArn acs:ram::5509671837805201:role/myrole --RoleSessionName service` 

As you can see you can also specify a role session name which is used as the caller id which is logged in ActionTrail for example.
Usually, you should restrict access to a specific group of users. An individual user can be specified as `acs:ram::5509671837805201:user/myuser`.

There are three principal types we currently support: 
- RAM user from a trusted Alibaba Cloud account. This what we have just discussed. Note that you can also specify RAM users from other accounts by simply changing the account id accordingly. This enables you to implement cross-account access permissions.
- Cloud services such as ECS, RDS, Function Compute, etc. This enables these services to automatically assume roles and thereby getting according permission when they are configured to access other cloud resources. 
- Identities from other Identity Providers such as Active Directory that are being used in Single Sign-On scenarios.  

### Resources
We have already briefly touched what a cloud resource is previously but let's recap: a cloud resource is any instance of a particular cloud service. For example, an ECS instance is a resource of the ECS service. Likewise, an OSS bucket and an OSS object is a resource of the OSS service.
Each resource on Alibaba cloud has a unique identified that you can use to define very fine granular permissions. Instead of granting full access to each and every ECS instance in your account you can only grant access to a particular ECS instance. This is where the `Resource`field of a RAM policy comes into play as already shown section [RAM Users and Policies](#ch-gov-users-and-policies). The identifier of an Alibaba cloud resource is always structured like this:
```
acs:<service-name>:<region-id>:<account-id>:<resource-name>
```
You can also specify a wildcard (*) expression for the individual parts of a resource. For example, if you like to specify all ECS instances in all regions you would write
```
acs:ecs:*:<account-id>:*
```
If you like to specify a specific object (myfile.dat) in a specific bucket (mybucket) in eu-central-1, you would write:
```
acs:oss:eu-central-1:<account-id>:mybucket/myfile.dat
```
 Since a bucket is always bound to a particular region you can also omit the region like so
```
acs:oss::<account-id>:mybucket/myfile.dat
```

### Best Practices


## Links
- The Official RAM documentation at https://www.alibabacloud.com/help/product/28625.htm which discusses many aspects of this chapter in more detail and also comes with a Tutorial section that focuses on many common scenarios by giving concrete examples.


[^aliyun]: https://github.com/aliyun/aliyun-cli
[^ossutil]: https://github.com/aliyun/ossutil
[^sdk]: https://github.com/aliyun?q=sdk

## Billing Management
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Regions and Availability Zones
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Service Level Agreements
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

