# Interacting with Alibaba Cloud
Alibaba Cloud provides different means to interact programmatically and to debug and troubleshoot your system.
Before we look at more detail at the various options let's first discuss the general API model of Alibaba Cloud since this is the very fundament on which every supporting technology is based on.

## API Model
Alibaba Cloud provides a web-based management API to manage the entire lifecycle and configuration of the various cloud resources available on our platform such as ECS and VPC. These management APIs are refered to as **OpenAPI**. They are only routable with public internet access.
In addition there is also the service specific endpoint which let you use the actual service's functionality such as executing a SQL query or writing data to an OSS bucket. This endpoint usually differs from the OpenAPI endpoint and is provided as an public endpoint routable from the public internet, and an internal endpoint which is only routable from a VPC. Please refer to the documentation of that particular service or look at the web-console of a provisioned service to get the endpoint schema.

Alibaba Cloud performs authentication on each access request. Therefore, each request, whether being sent by HTTP or HTTPS, must contain signature information. Every service performs symmetric encryption using the `Access Key ID` and `Access Key Secret` to authenticate the client's request. Both keys are issued by the service *Resource Access Management Service (RAM)*. The `Access Key ID` indicates the identity of the client. The `Access Key Secret` is the secret key used to both encrypt and verify the signature string on the client side and on the server, respectively. 
A detailed example based on RDS can be found here: https://www.alibabacloud.com/help/doc-detail/26225.htm

Alibaba Cloud APIs are either exposed as RPC-style endpoints or REST-based endpoints. Which style is used depends on the service and can be looked up at the API documentation of the particular service. 
ECS for example uses RPC-style, while Container Registry uses REST-based style.

- RPC-based APIs have the following format: https://Endpoint/?Action=xx&Parameters
So if you need to query one or more VPCs, the action is `DescribeVpcs`

- REST-based APIs follow the (surprise!) REST principles, e.g. GET https://Endpoint/resource/

The endpoint is usually structured like this:
`{service abbreviation}.{regionId}.aliyuncs.com`
but there are some exceptions to that. For instance the Object Storage Service is structured like this
`mybucket.oss-{regionId}.aliyuncs.com`

For each service there is always a default endpoint `{service abbreviation}.aliyuncs.com`, and a region specific endpoint as described above. From a functional perspective it does not matter which one you are going to call, but network-latency wise it does. So make sure to always call the endpoint in the region which is closest to your system, and avoid calling the standard endpoint since this is usually located in Singapore region or in some rare cases in Chinese regions which come with high latency and an unreliable network connection.

The endpoints we just discussed are exposed to the public internet. In many scenarios, however, communication happens from inside a Virtual Private Network (VPC) with no outbound (inbound) internet access for security and isolation reasons. Calling these public endpoints in such environments with no outbound internet access is thus not possible.

Both HTTP and HTTPS are supported for all endpoints. We recommend to send requests over HTTPS for a higher level of security.

Now that we have taken a look at how Alibaba Cloud Management APIs work let's discuss important technologies and methodologies that built upon that.

## Using the command-line interface
The Alibaba Cloud CLI is a tool to manage and use Alibaba Cloud resources through a command line interface. It is written in Go and built on the top of Alibaba Cloud OpenAPI. It is developed and hosted on Github at https://github.com/aliyun/aliyun-cli. CLI access the Alibaba Cloud services through OpenAPI (see previous section). Before using Alibaba Cloud CLI, make sure that you have activated the service (see chapter 2) to use and known how to use OpenAPI. Also make sure that your machine has public internet access (e.g. through a NAT-Gateway, Elastic IP/Instance-bound IP), otherwise OpenAPI cannot be called.

Aliyun CLI provides an interactive configuration experience by running
```
$ aliyun configure
Configuring profile 'default' ...
Aliyun Access Key ID [None]: <Your AccessKey ID>
Aliyun Access Key Secret [None]: <Your AccessKey Secret>
Default Region Id [None]: eu-central-1
Default output format [json]: json
Default Language [en]: en
```

This command will create the file `$HOME/.aliyun/config.json` which you can also manually create, of course.
It is structured like this:

```
{
	"current": "default",
	"profiles": [
		{
			"name": "default",
			"mode": "AK",
			"access_key_id": "LTAI4FxdfaoqTCJWKi******",
			"access_key_secret": "Hv1PvFJHiYQKbES6wB8jrFIW******",
			"sts_token": "",
			"ram_role_name": "",
			"ram_role_arn": "",
			"ram_session_name": "",
			"private_key": "",
			"key_pair_name": "",
			"expired_seconds": 0,
			"verified": "",
			"region_id": "eu-central-1",
			"output_format": "json",
			"language": "en",
			"site": "",
			"retry_timeout": 0,
			"retry_count": 0
		}],
	"meta_path": ""
}
```

You can add multiple profiles with different configurations. 
Switching between profiles is as easy as
```
aliyun configure set --profile <profile-name>
```
Environment variables always take precedence over the configuration file. The following variables are considered:
```
ALIBABACLOUD_REGION_ID > ALICLOUD_REGION_ID > REGION    
ALIBABACLOUD_ACCESS_KEY_ID > ALICLOUD_ACCESS_KEY_ID > ACCESS_KEY_ID 
ALIBABACLOUD_ACCESS_KEY_SECRET > ALICLOUD_ACCESS_KEY_SECRET > ACCESS_KEY_SECRET  
```

Note that your credentials are stored in plain-text so make sure that access to this directory is properly secured and use it in combination with the open-source tool [Alicloud-Vault](#ch_interacting_vault). So especially when running on ECS instances we do not recommend to use the Access Key mode (AK) of Aliyun CLI. Instead, we recommend to use the mode `EcsRamRole` which instructs the Aliyun CLI to get an STS-token from the metadata service and thus assume the role that was assigned to that particular ECS instance.
Assuming that the role named *ecs_role* is to be assumed you would configure the CLI interactively as follows:
```
$ aliyun configure --mode EcsRamRole --profile myprofile
``` 
See https://github.com/aliyun/aliyun-cli#configure-authentication-methods for details.

Last but not least the Aliyun CLI command and usage structure is as follows:
```
$ aliyun <product> <api> [--parameter1 value1 --parameter2 value2 ...]
```

Putting `help` after either the *product* or *api* gives you detailed information about the available actions and required and optional parameters. A simple
```
$ aliyun help
```
will print out all available services.

Aliyun CLI take the following precende


{id: ch_interacting_vault}
## Alicloud-Vault
Alicloud-Vault is another handy tool for many development scenarios. It is open-source and developed at https://github.com/arafato/alicloud-vault.
Its is a vault for securely storing and accessing Alibaba Cloud credentials in development environments and takes many inspirations from https://github.com/99designs/aws-vault. 

Alicloud-Vault stores RAM credentials in your operating system's secure keystore and then generates temporary credentials from those to expose to your shell and applications. It's designed to be complementary to the Aliyun CLI tools, and is aware of your config file and profiles in `~/.aliyun/config`.

It uses Alibaba Cloud's STS service to generate temporary credentials via the AssumeRole API call. These expire in a short period of time, so the risk of leaking credentials is reduced. Note that not all services support STS. You can find the currently supported services here: https://www.alibabacloud.com/help/doc-detail/135527.htm

Alicloud-Vault then exposes the temporary credentials to the sub-process through environment variables in the following way

```
$ alicloud-vault exec jonsmith -- env | grep ALICLOUD
ALICLOUD_VAULT=jonsmith
ALICLOUD_REGION_ID=us-east-1
ALICLOUD_ACCESS_KEY_ID=%%%
ALICLOUD_ACCESS_KEY_SECRET=%%%
ALICLOUD_STS_TOKEN=%%%
ALICLOUD_SESSION_EXPIRATION=2020-03-06T10:02:33Z
```

Please refer to the official site at https://github.com/arafato/alicloud-vault for detailed information on how to use it and configure it properly.

## Using the OSSUtil CLI
While `Aliyun CLI` gives you great control over a wide array of services for OSS Alibaba Cloud is providing a dedicated command line utility called `ossutil` which is hosted on Github at https://github.com/aliyun/ossutil.
It provides *both* convenient access to the *OpenAPI* and fine-grained control to the *Service API* of OSS which allows you to generate signed URLs, disabling CRC64 during data transmission, etc.  

The configuration file is stored in `$HOME/.ossutilconfig`. If you need to store in a different place you can so by using the `-c` option with which you can specify a custom configuration file path.
A configuration file has the following structure: 
```
[Credentials]
        language = EN
        endpoint = oss.aliyuncs.com
        accessKeyID = your_key_id
        accessKeySecret = your_key_secret
        stsToken = your_sts_token
[Bucket-Endpoint]
        bucket1 = endpoint1
        bucket2 = endpoint2
        ...
[Bucket-Cname]
        bucket1 = cname1
        bucket2 = cname2
        ...
[AkService]
        ecsAk=http://100.100.100.200/latest/meta-data/Ram/security-credentials/<your RAM role name>
```

Let's break down this structure:
- The **Credentials** section define the default settings that should be used for every call unless otherwise specified (see below sections). The *endpoint* option overrides the default OSS endpoint. By default, the Internet address `oss.aliyuncs.com` directs to the Internet endpoint of China East 1 (Hangzhou), and the intranet address `oss-internal.aliyuncs.com` directs to the intranet endpoint of China East 1 (Hangzhou).
- The **Bucket-Endpoint** section lets you define endpoints for specific buckets. This is needed if the location of your bucket differs from your default endpoint you have specified in the Credentials section (or from the default endpoint if you haven't specified an endpoint at all).
- The **Bucket-Cname** section lets you define CNAMEs for your bucket endpoints. This is especially usefull and needed if you are exposing your buckets through a Content Delivery Network (CDN) service which you like to use in combination with `ossutil`, for example.
- The **AkService** section is required if you need to use a RAM role bound to an ECS instance to perform operations on OSS. When you configure this option, you only need to set *ecsAK* to the RAM role bound to the ECS instance. After configuring the AkService option, you do not need to configure the accessKeyID, accessKeySecret, and stsToken options. If these options are configured, the configurations of these options instead of the AkService option take effect. 

The priority of endpoint selection is defined as follows: `--endpoint > Bucket-Cname > Bucket-Endpoint > endpoint > default OSS endpoint``
That is, CLI option takes precedence over Bucket-Cname takes precedence over Bucket-Endpoint, ...,  

## Infrastructure as Code
Wikipedia defines Infrastructure as Code (IaC) as follows:
```
 Infrastructure as code is the process of managing and provisioning computer data centers through machine-readable definition files, rather than physical hardware configuration or interactive configuration tools.
```
Or to put it in simpler terms:
```
Infrastructure as Code (IaC) means to manage the entire lifecycle and configuration of your IT infrastructure using (declarative) configuration files.
```

With IaC, your infrastructure’s configuration takes the form of a code file which uses a declarative description. *Declarative* means you are solely defining the desired state, not how to get to this state. This is usually taken care of by the IoC-Technology such as Terraform and thus much less error-prone and readable and understandable.
Since it’s just text, it’s easy for you to edit, copy, and distribute it. You can *and should* put it under source control, like any other source code file.

### Benefits
Let us now dive into some of the benefits your organization will reap by adopting an IaC solution in combination with cloud-based technologies.

- **Speed**
The first significant benefit IaC provides is speed. Infrastructure as code enables you to quickly set up your complete infrastructure by running a script. You can do that for every environment, from development to production, passing through staging, QA, and more. IaC can make the entire software development lifecycle more efficient.

- **Consistency**
Manual processes result in mistakes, period. Humans are fallible. Our memories fault us. Communication is hard, and we are in general pretty bad at it. As you’ve read, manual infrastructure management will result in discrepancies, no matter how hard you try. IaC solves that problem by having the config files themselves be the single source of truth. That way, you guarantee the same configurations will be deployed over and over, without discrepancies.

- **Accountability**
This one is quick and easy. Since you can version IaC configuration files like any source code file, you have full traceability of the changes each configuration suffered. No more guessing games about who did what and when.

- **Higher Efficiency** 
By employing infrastructure as code, you can deploy your infrastructure architectures in many stages. That makes the whole software development lif cycle more efficient, raising the team’s productivity to new levels.
You could have programmers using IaC to create and launch sandbox environments, allowing them to develop in isolation safely. The same would be true for QA professionals, who can have perfect copies of the production environments in which to run their tests. Finally, when it’s deployment time, you can push both infrastructure and code to production in one step.

- **Lower Cost**
One of the main benefits of IaC is, without a doubt, lowering the costs of infrastructure management. By employing cloud computing along with IaC, you dramatically reduce your costs. That’s because you won’t have to spend money on hardware, hire people to operate it, and build or rent physical space to store it. But IaC also lowers your costs in another, subtler way, and that is what we call “opportunity cost.”

Every time you have smart, high-paid professionals performing tasks that you could automate, you’re wasting money. All of their focus should be on tasks that bring more value to the organization. And that’s where automation strategies—infrastructure as code among them—come in handy. By employing them, you free engineers from performing manual, slow, error-prone tasks so they can focus on what matters the most.

### Terraform
Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently. Terraform can manage existing and popular service providers as well as custom in-house solutions.
It is hosted at https://www.terraform.io/

Configuration files describe to Terraform the components needed to run a single application or your entire datacenter. Terraform generates an execution plan describing what it will do to reach the desired state, and then executes it to build the described infrastructure. As the configuration changes, Terraform is able to determine what changed and create incremental execution plans which can be applied.

The infrastructure Terraform can manage includes low-level components such as compute instances, storage, and networking, as well as high-level components such as DNS entries, SaaS features, etc.

Terraform is extensible and thus supports a wide range of different cloud vendors. The official Alibaba Cloud Terraform code-repository is hosted at https://github.com/terraform-providers/terraform-provider-alicloud. The complete documentation can be found at https://www.terraform.io/docs/providers/alicloud/. So while you cannot re-use the same template code across different cloud providers you can re-use your general Terraform knowledge. 

We highly recommend to look at the official examples provided at https://github.com/terraform-providers/terraform-provider-alicloud/tree/master/examples to get started easily.
For example, the following script defines the desired state of a VPC with a certain network range you would like to create:
```
provider "alicloud" {
}

resource "alicloud_vpc" "main" {
  name       = "my-vpc"
  cidr_block = "192.168.0.0/16"
}
```
The Terraform CLI then provides a huge set of options to manage the entire state and lifecycle of your resources and configurations which are described in your template code.

### Resource Orchestration Service
Resource Orchestration Service (ROS) is Alibaba Cloud's native IoC offering. It provides support for the most important services and features of Alibaba Cloud and will extend support even further in the near future. 
Just like with Terraform you build a template and use it to create all of the necessary resources, collectively known as a ROS stack. This model removes opportunities for manual error, increases efficiency, and ensures consistent configurations over time.
You can either store check out your ROS Templates locally to your machine or upload them to Alibaba Cloud Object Storage Service (OSS) and then use ROS via the browser console, *Aliyun CLI*, or APIs to create a stack based on your template code.
ROS aims to provide the best possible support for Alibaba Cloud services and features. Its application is, however, solely bound to Alibaba Cloud. Knowledge cannot be re-used across different cloud providers, albeit that it shares many concepts and its syntax with AWS Cloud Formation.

The following ROS script defines the desired state of a VPC with a certain network range:
```
{
  "ROSTemplateFormatVersion": "2015-09-01",
  "Resources": {
    "EcsVpc": {
      "Type": "ALIYUN::ECS::VPC",
      "Properties": {
        "VpcName": "my-vpc",
        "CidrBlock": "192.168.0.0/16"
      }
    }
  }‚
}
```

The *Aliyun CLI* lets you then manage the entire state and lifecycle of your resources and configurations which are described in your template code. You will find detailed information on how to use the *Aliyun CLI* with ROS here: https://www.alibabacloud.com/help/doc-detail/137399.htm

## Debugging with API-Explorer
Sometimes you want to be able to call and explore the OpenAPI easily using a convenient graphical interface. This is where OpenAPI Explorere comes into play which you can find at https://api.aliyun.com.
Make sure to have an active login session running in your browser since the OpenAPI Explorer will use the Access Key of the current user to temporarily access and operate on the user’s resources. This means that permission-wise you are of course restricted to your assigned permissions.
Below figure gives you an idea on how OpenAPI Explorer looks like.
![Interacting - OpenAPI Explorer](03/openapi_explorer.png) 

As you can see you explore and search for specific actions and immediately get a graphical interface which lets you conveniently specify all parameters. It also gives you a nice graphical representation of the resulting request and response values, which of course is great for debugging and troubleshooting.
What is also very practical is the *Example Code* section for different programming languages such as Java, NodeJS, Go, etc. that instantly generates the code that will create and submit the according action you have selected.

One of the highlights is the Data Simulation feature. It lets you automatically create a Mock endpoint with mocked return values. As such it simulates the results of a real OpenAPI call, which you can use to replace the real OpenAPI request address in a development environment and lets you simulate different data scenarios of OpenAPI.

The mock endpoints take the following form:
```
https://api.aliyun.com/mock/<service/<action>
```

So for mocking the `DescribeTasks` of ECS service you can simply call
```
https://api.aliyun.com/mock/Ecs/DescribeTasks
```
which returns according mock data. No authentication and signing of the request is needed for calling the mock API.