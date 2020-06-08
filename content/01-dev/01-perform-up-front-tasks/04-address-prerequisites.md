---
title: "Address Prerequisites"
pre: "4. "
disableToc: true
weight: 40
---

{{% comment %}}
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: CC-BY-SA-4.0
{{% /comment %}}

In this step your foundation team will address several prerequsites before you build out the environment.

{{< toc >}}

## 1. Determine Whether to Reuse Existing Master AWS Account

Later in this guide you'll be using AWS Control Tower to set up an initial landing zone or basis of your AWS environment.  Since AWS Control Tower supports reusing existing master AWS accounts and using newly created master AWS accounts, you need to decide which option best suites your needs.

Review [Plan Your AWS Control Tower Landing Zone](https://docs.aws.amazon.com/controltower/latest/userguide/planning-your-deployment.html) for guidance when considering whether or not to use an existing master AWS account.

If you have any doubts about the quality of an existing master AWS account, then it's recommended that you create a new master AWS account later in this guide to support your new AWS environment.

## 2. Create Email Addresses for New AWS Accounts

Regardless as to whether you create a new master AWS account or reuse an existing master AWS account, you'll need to prepare a set of email addresses to represent the root user of each of the new AWS accounts that will be created. In later steps, when you create AWS accounts, you'll be referring to these email addresses. Each AWS account must have a unique email address associated with it.

### Use Either Email Distribution Lists (DLs) or Shared Mailboxes
Instead of using a person's email address, it's recommended that you use either email distribution lists (DLs) or shared mailboxes so that you can enable at least several trusted people in your organization, for example, your Cloud Administrators, access to email messages associated with each AWS account.

### Carefully Control Access to the Email Accounts
Since the email address associated with an AWS account is used as the [root user login for the account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html), anyone with access to that email account will have access to password reset process for the account. 

### Define and Request DLs or Shared Mailboxes
The following table includes the minimum set of email addresses to get started. Each AWS account must have a globally unique email address. 

If your organization already has a naming standard for mail addresses associated with services, you should use that standard format and include references to at least "aws" and and an abbreviation of the unique role or purpose of each account.

Use your organization's process to request either DLs or shared mailboxes based on set of addresses you identify.

{{% notice tip %}}
**Reusing existing master AWS account:** If you're reusing an existing master AWS account, you'll reuse the email address associated with that account.
{{% /notice %}}

|AWS Account	|Example Email Address|Example with "+" Style Email Address|
|---|---|---|
|**Foundation AWS Accounts**||
|Master (only needed when creating a new master AWS account)|aws-account-master@example.com|aws-account+master@example.com|
|Audit|aws-account-audit@example.com|aws-account+audit@example.com|
|Log Archive|aws-account-log-archive@example.com|aws-account+log-archive@example.com|
|Network Production|aws-account-network-prod@example.com|aws-account+network-prod@example.com|
|**Team Development AWS Accounts**|||
|Foundation Team Development|aws-account-foundation-dev@example.com|aws-account+foundation-dev@example.com|
|Builder Team 1 Development|aws-account-team-a-dev@example.com|aws-account+team-a-dev@example.com|

{{% notice tip %}}
**Use of “+” style email addresses:** If your organization’s email system supports the use of “+” style email addresses in which email multiple email addresses are aliased to the same email account, then you might find it beneficial to use this form to consolidate the root user email addresses for either all or collections of AWS accounts to either one or a few actual email accounts. For example: [aws-account1+master@example.com](mailto:aws-account1+master@example.com) and [aws-account1+audit@example.com](mailto:aws-account1+audit@example.com) will be treated as unique addresses in AWS but your mail system may deliver the mail to the same [aws-account1@example.com](mailto:aws-account1@example.com) email address.
{{% /notice %}}

{{% notice tip %}}
**Office 365 Customers:** It appears that plus style addressing is on the [Office 365 roadmap for 2020](https://techcommunity.microsoft.com/t5/exchange-team-blog/exchange-transport-news-from-microsoft-ignite-2019/ba-p/993417).
{{% /notice %}}

## 3. Obtain Non-Overlapping IP Address Range

In this step you should consult with your existing Network team to obtain a suitably sized, non-overlapping IP address range or [CIDR block](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) that can be used not only for the initial shared development network that will be set up in this guide, but also to accomodate pre-production test and and production networks that you will provision on AWS as you progress in your journey.

Since your organization will likely interconnect at least a portion of your on-premises networks to your emerging AWS hosted networks, a best practice is to assign a large IP address range or CIDR block for use in AWS that does not overlap with your existing allocated IP addresses. By using non-overlapping IP address ranges, your organization will avoid needing to introduce a complicated network address translation (NAT) solution.

### Recommended IP Address Range Size

Ideally, taking into account future networks beyond the initial development network, you should obtain for your organization's use of AWS overall, an IP address range or CIDR block of at least size `/18` to `/16`.

If the desired sizes of non-overlapping CIDR block cannot be obtained at this stage, you should obtain a block of at least size `/22` to address the initial shared development network.  You can obtain additional non-overlapping CIDR blocks later to support your build out of pre-production test and production networks.

{{% notice warning %}}
**Larger CIDR Range is Better:** Although 1,000 IP addresses may sound like a lot, don't assume that it's a sufficiently sized range even for your initial few workloads.  Since your initial shared development environment will likely have at least 4 subnets, when you divide a `/22` CIDR block across the 4 subnets, you end up with only 254 IP available addresses per subnet. Using a `/23` block would leave only 126 IP addresses per subnet. Depending on the number of workloads your infrastructure and workload builder teams will be experimenting and testing, these smaller ranges can end up being exhausted faster than you might expect.
{{% /notice %}}

### Unable to Obtain Non-Overlapping IP Address Range

If you cannot obtain a non-overlapping CIDR block at this stage, you can temporarily use an overlapping block for your initial shared development network. 

In the future, when you need to interconnect a portion of your existing on-premises network, you will need to create a new VPC with a non-overlapping CIDR block, migrate the workloads to the new VPC, and decommission the old VPC.

### Resources

* [VPC and Subnet Sizing for IPv4](https://docs.aws.amazon.com/vpc/latest/userguide//VPC_Subnets.html#vpc-sizing-ipv4)

* [Visual Subnet Calculator](http://www.davidc.net/sites/default/subnets/subnets.html)

## 4. Decide on Organizational Identifier

Since you will be assigning names to several cloud resources while you follow the steps in this guide, it's useful for you to decide on a unique organizational identifier to assign as a prefix to those names so that:
* Names of your resources won't collide with other names when working in global namespaces.
* You'll be able to create security policies to restrict access based on resources with that prefix.

Your organization is likely already using a stock ticker or other similar abbreviation to represent your organization in support of current business and IT processes.  One of these existing identifiers might be appropriate to carry forward as you build out your cloud environment.

You should limit the length of the abbeviated organization identifier to a handful of characters. Using no more than 4-5 characters is a good guide.

As an example, this guide uses the prefix **`example`** throughout. As you progress through the guide, you're expected to replace this example prefix with your organization's own identifier.

{{% notice tip %}}
**More Extensive Resource Naming Standards:** As you progress on your journey, you may find it useful to adopt more extensive cloud resource naming standards.
{{% /notice %}}

## 5. Identify Your Preferred AWS Region

When you build out the foundation of your AWS environment using the AWS Control Tower service, you will need to specify a “home” AWS region in which AWS Control Tower will configure a set of resources. Typically, this AWS region will be the AWS region in which you expect to host most of your workloads.  See [AWS Regions and Availability Zones](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/) for a list of the current regions.