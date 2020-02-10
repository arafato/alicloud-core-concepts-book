# Governance
Cloud governance is the development and implementation of controls to manage access, budget, and policies for security, compliance, and even high-availability and resiliency across your workloads in the cloud. In this chapter we will focus on the core concepts and according best-practices to implement according controls and policies for most of the before mentioned aspects. 

## Account Management
There are currently two different types (or *memberships*) of Alibaba Cloud accounts: *Individual Account* and *Enterprise Account*. There is no difference in terms of functionality. They differ, however, in terms of real-name verification requirements, free tier offering, discount eligibility, and whether they can be added to an Resource Directory account. Let's quickly go through each of them. 
- **Real-Name Verification:** Each account needs to be verified with a real identity if you want to create cloud resources in Mainland China. This is a strict requirement. Resources include OSS buckets and ECS instances for example. Basically, all resources that either expose a public IP address or a public domain name. The verification differs from what kind of identity proof needs to be submitted for each account type. For *Individual Account* you need to submit a proof of your personal identity which could be a copy of your passport. For *Enterprise Account* you need to submit an excerpt from the commercial register. This process is completely supported by the Web Portal and usually takes 3-4 business days to complete.
- **Free-Tier Offering:** Based on the account type the free trial offering differs in terms of how many free credits you get and what kind of products you are eligible for. Please see [https://www.alibabacloud.com/campaign/free-trial](https://www.alibabacloud.com/campaign/free-trial) for details. The *Enterprise Account* has a much bigger free product range and also more free credits.
- **Discount Eligibility:** Throughout the year Alibaba Cloud often has special promotion campaigns which include discounts on certain products. These discounts often differ depending on the cloud account type. Usually, *Enterprise Accounts* are eligible for higher discounts.
- **Resource Directory Integration** For Multi-Account management, Alibaba Cloud is providing a service called Resource Directory. It allows to manage multiple accounts under one master account and to aggregate metrics, logs, RAM users, and billing data for example. As of this writing it has not been released in the INTL Portal yet. One it has been released you can programmatically (i.e. API-based) add new accounts or add existing *Enterprise Accounts*. Personal Accounts are not supported.

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

In the [best-practices section of User and Permission Management](#ch-gov-users-and-policies-bp) we will look at more recommendations to keep your cloud resources secure. 

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
- Real-name verification of your account which is needed for creating any kind of resources in Mainland China regions which have a public IP and/or a public domain name.

Most other things (such as whitelisting port 25 for an ECS instance, Reverse DNS entry for ECS) are usually requested by opening an according support ticket which is not restricted to the root user, though.

{id: ch-gov-users-and-policies}
### RAM Users and Policies:
A RAM user is sometimes also referred to as *Sub-Account*. It is a user account that is used for web-based login to the Alibaba Cloud portal and/or programmatic access to the OpenAPI. They can't access anything in your account until you give them permission. All permissions need to be explicitly granted.
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

Below is an example role definition that allows *every* RAM user (yes, this is a little bit counter-intuitive since we specify `root` as the principal) to assume it who is allowed to invoke `sts:AssumeRole` and whose request originates from within a certain IP range. 
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
If you like to specify a specific object such as `myfile.dat` in a specific bucket `mybucket` in `eu-central-1`, you would write:
```
acs:oss:eu-central-1:<account-id>:mybucket/myfile.dat
```
 Since a bucket is always bound to a particular region you can also omit the region like so
```
acs:oss::<account-id>:mybucket/myfile.dat
```
{id: ch-gov-users-and-policies-bp}
### Best Practices
To help secure your Alibaba Cloud account follow these recommendations for Alibaba Cloud Resource Access Management:
- **Lock Away Your Root Credentials:** <br>
You use an access key (an access key ID and secret access key) to make programmatic requests to Alibaba Cloud. However, do not use your root user access key. The access key for your root user gives full access to all your resources for all Alibaba Cloud services, including your billing information. You cannot reduce the permissions associated with your Alibaba Cloud account root user access key.

Therefore, protect your root user access key like you would your credit card numbers or any other sensitive secret. Unless you do not absolutely need a root access keys never create them in the first place. If you do have one, delete them. You can check at [https://usercenter.console.aliyun.com](https://usercenter.console.aliyun.com) whether according keys have been defined.

- **Create Individual RAM Users:**<br>
Don't use your Alibaba Cloud account root user credentials to access any cloud service or resource, and don't give your credentials to anyone else. Instead, create individual users for anyone who needs access to your cloud account. Create a RAM user for yourself as well, give that user administrative permissions, and use that RAM user for all your work.<br>
By creating individual RAM users for people accessing your account, you can give each RAM user a unique set of security credentials. You can also grant different permissions to each RAM user. If necessary, you can change or revoke a RAM user's permissions anytime. If you give out your root user credentials, it can be difficult to revoke them, and it is impossible to restrict their permissions.

- **Use Groups to Assign Permissions to RAM Users:**<br>
Instead of defining permissions for individual RAM users, it's usually more convenient to create groups that relate to job functions (administrators, developers, accounting, etc.). Next, define the relevant permissions for each group. Finally, assign RAM users to those groups. All the users in an RAM group inherit the permissions assigned to the group. That way, you can make changes for everyone in a group in just one place. As people move around in your company, you can simply change what RAM group their RAM user belongs to.

- **Grant Least Privilege:**<br>
When you create IAM policies, follow the standard security advice of granting least privilege, or granting only the permissions required to perform a task. Determine what users (and roles) need to do and then craft policies that allow them to perform only those tasks.
Start with a minimum set of permissions and grant additional permissions as necessary. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later.<br>
Alibaba Cloud RAM service comes with a set of pre-defined policies which are called *System Policies*. You can find them in your cloud account portal here: https://ram.console.aliyun.com/policies<br>
Usually, for each service there are two policies defined: One that gives full access, and one that only gives read access to any resource in any region. While this may work for some scenarios you might need more fine-granular access policies that are specific to certain resource, to a certain region, or to some specific actions. For these scenarios you can define a so-called *Custom Policy*.<br>
Some built-in system policies you might want to consider (in potentially modified versions) for your account governance are: 
    - *AliyunSupportFullAccess* which grants access to Support Center via Management Console which includes filing support tickets. 
    - *AliyunBSSFullAccess* which grants full access to Billing System via Management Console.
    - *AliyunBSSReadOnlyAccess* read-only access to Billing System via Management Console.
    - *AliyunBSSOrderAccess* which grants permission to view, pay, and cancel orders on Billing System.
    - *AliyunSTSAssumeRoleAccess* which grants access to the API AssumeRole of Security Token Service(STS).
    - *AliyunRAMFullAccess* which grants full access to the RAM service which includes rights to create, modify and delete users, and specify account policies such as password policies. 
    - *AliyunNotificationsFullAccess* which grants full access to Alibaba Message Center via Management Console.
    - *AliyunMarketplaceFullAccess*  which grants full access to Alibaba Cloud Marketplace via Management Console.
    - *AliyunBeianFullAccess* which grants full access to the Alibaba Cloud ICP Filing system at https://beian.aliyun.com

- **Configure a Strong Password Policy for Your RAM-Users:**<br>
If you allow users to change their own passwords, require that they create strong passwords and that they rotate their passwords periodically. On RAM Settings page of the Alibaba Cloud portal at https://ram.console.aliyun.com/settings, you can create a password policy for your account. You can use the password policy to define password requirements, such as minimum length, whether it requires non-alphabetic characters, how frequently it must be rotated, password history checks, and so on.

- **Enable MFA**<br>
For extra security, we recommend that you require multi-factor authentication (MFA) for all users in your account. With MFA, users have a device that generates a response to an authentication challenge. Both the user's credentials and the device-generated response are required to complete the sign-in process. If a user's password or access keys are compromised, your account resources are still secure because of the additional authentication requirement.<br>
Please check https://www.alibabacloud.com/help/doc-detail/119555.htm for details.

- **Use Service-Roles for Applications That Run on Alibaba Cloud Services:**<br>
Applications that run on Alibaba Cloud services such as ECS instances or Function Compute need credentials in order to access other Alibaba Cloud services. To provide credentials to the application in a secure way, use RAM service-roles. A service-role is an entity that has its own set of permissions, but that isn't a user or group. Roles also don't have their own permanent set of credentials the way RAM users do. In the case of Function Compute for example, RAM dynamically provides temporary credentials to the Function Compute instance, and these credentials are automatically rotated for you.<br>
When you launch an ECS instance, you can specify a role for the instance as a launch parameter. Applications that run on the ECS instance can use the role's credentials when they access other cloud resources through Secure Token Service (https://www.alibabacloud.com/help/doc-detail/28756.htm). The role's permissions determine what the application is allowed to do.

- **Use Roles to Delegate Permissions:**<br>
Don't share security credentials between accounts to allow users from another Alibaba Cloud account to access resources in your cloud account. Instead, use RAM roles. You can define a role that specifies what permissions the RAM users in the other account are allowed. You can also designate which Alibaba Cloud accounts have the RAM users that are allowed to assume the role. <br>
See https://www.alibabacloud.com/help/doc-detail/93745.htm for details on how to configure accordingly.

- **Do Not Share Access Keys:**<br>
Access keys provide programmatic access to Alibaba Cloud. Do not embed access keys within unencrypted code or share these security credentials between users in your Alibaba Cloud account. For applications that need access to Alibaba Cloud, configure the program to retrieve temporary security credentials using a RAM role. To allow your users individual programmatic access, create a RAM user with personal access keys.

- **Rotate Credentials Regularly:**<br>
Change your own passwords and access keys regularly, and make sure that all RAM users in your account do as well. That way, if a password or access key is compromised without your knowledge, you limit how long the credentials can be used to access your resources. You can apply a password policy to your account to require all your RAM users to rotate their passwords. You can also choose how often they must do so.

- **Use Policy Conditions for Extra Security:**<br>
To the extent that it's practical, define the conditions under which your RAM policies allow access to a resource. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from. You can also specify that a request is allowed only within a specified date range or time range. You can also set conditions that require the use of MFA (multi-factor authentication). For example, you can require that a user has authenticated with an MFA device in order to be allowed to terminate an ECS instance.
Please see https://www.alibabacloud.com/help/doc-detail/100680.htm#h2-url-8 for details on the syntax of the `Condition` field of an according RAM policy.

- **Enable ActionTrail:**<br>
You can use the Alibaba Cloud service *ActionTrail* to determine the actions users have taken in your account and the resources that were used. The log files show the time and date of actions, the source IP for an action, which actions failed due to inadequate permissions, and more. Keep in mind that these logs are exclusively from the OpenAPI, i.e., the management APIs of Alibaba Cloud. Application specific logs can be recorded and analyzed with *Log Service*, for example. 


## Links
- The Official RAM documentation at https://www.alibabacloud.com/help/product/28625.htm which discusses many aspects of this chapter in more detail and also comes with a Tutorial section that focuses on many common scenarios by giving concrete examples.
- The official ActionTrail documentation at https://www.alibabacloud.com/help/product/28802.htm which explores various scenarios such as security analysis, resource change tracking, and compliance audit. 
- The official Log Service documentation at https://www.alibabacloud.com/help/product/28958.htm which discusses how to collect, store and analyze logs from your applications.

[^aliyun]: https://github.com/aliyun/aliyun-cli
[^ossutil]: https://github.com/aliyun/ossutil
[^sdk]: https://github.com/aliyun?q=sdk

## Billing Management
Alibaba Cloud Billing Management is the service that you use to pay your Alibaba Cloud bill, monitor your usage, and budget your costs.

Alibaba Cloud automatically charges the credit card you provided when you signed up for a new account with Alibaba Cloud. Charges appear on your credit card bill monthly. You can view or update credit card information, and designate a different credit card for Alibaba Cloud to charge, on the Payment Methods page in the Billing Management console.

Depending on the region you choose during initial account creation, you are contracting with a different legal identity of Alibaba Cloud. Outside of Mainland China we are providing the following legal entities for contracting:
- Alibaba.com (Europe) Limited
- Alibaba Cloud (Singapore) Private Limited
- Alibaba Cloud US LLC
- Alibaba Cloud (India) LLP
- Alibaba Cloud (Malaysia) Sdn. Bhd

Be careful in choosing the region. It cannot be changed afterwards and your contracting legal entity depends on it.

![Billing Management - Billing Address](02/billing_address.png)   

### Payment Methods
If you choose U.S. Dollars as payment currency, we support payment by credit card, debit card, or PayPal. If you choose Indian Rupee as payment currency, we support payment by Paytm Wallet. If you choose Malaysian Ringgit as payment currency, we support payment by credit card or debit card. Each account may have multiple payment methods registered at the same time, but there can only be one default payment method to be used for all your payments. For example, if you have used a credit card to pay for a prepaid service, this credit card will be your default payment method and you can only use it to make other purchases. You can only use another registered payment method to make payments if you do not have any other existing products or services currently being billed to another payment method in effect.

### Payment Failure
If, for reasons attributed to you or your registered payment method, we cannot bill you or otherwise process your payment, we will notify you by email to your registered email address and request you to resolve the problem. In this case, you will be able to continue to use your products for another 15 days. After this 15-day period, if the issue has not been resolved and payment has not been made, Alibaba Cloud shall have the right, to suspend your service until payment has been processed or to terminate your services without any liability to you. Prior to any service suspension or termination, an email will be sent to your registered email address.

After the termination of your services, Alibaba Cloud shall have the right, to release all your instances without any liability to you 15 days after the date of termination of your services.
