# Interacting with Alibaba Cloud
Alibaba Cloud provides different means to interact programmatically and to debug and troubleshoot your system.
Before we look at more detail at the various options let's first discuss the general API model of Alibaba Cloud since this is the very fundament on which every supporting technology is based on.

## API Model
Alibaba Cloud provides a web-based management API to manage the entire lifecycle and configuration of the various cloud resources available on our platform such as ECS and VPC.

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

Both HTTP and HTTPS are supported for all endpoints. We recommend to send requests over HTTPS for a higher level of security.

Now that we have taken a look at how Alibaba Cloud Management APIs work let's discuss important technologies and methodologies that built upon that.
## Infrastructure as Code
Wikipedia defines Infrastructure as Code (IaC) as follows:
```
 Infrastructure as code is the process of managing and provisioning computer data centers through machine-readable definition files, rather than physical hardware configuration or interactive configuration tools.
```
Or to put it in simpler terms:
```
Infrastructure as Code (IaC) means to manage the entire lifecycle and configuration of your IT infrastructure using declarative scripts.
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

Lower Cost
One of the main benefits of IaC is, without a doubt, lowering the costs of infrastructure management. By employing cloud computing along with IaC, you dramatically reduce your costs. That’s because you won’t have to spend money on hardware, hire people to operate it, and build or rent physical space to store it. But IaC also lowers your costs in another, subtler way, and that is what we call “opportunity cost.”

You see, every time you have smart, high-paid professionals performing tasks that you could automate, you’re wasting money. All of their focus should be on tasks that bring more value to the organization. And that’s where automation strategies—infrastructure as code among them—come in handy. By employing them, you free engineers from performing manual, slow, error-prone tasks so they can focus on what matters the most.

### Terraform

### Resource Orchestration Service 

## Using the command-line interface
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Debugging with API-Explorer
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.

## Programming with the SDK
Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet. Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea takimata sanctus est Lorem ipsum dolor sit amet.
