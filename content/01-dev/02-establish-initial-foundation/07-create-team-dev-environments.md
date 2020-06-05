---
title: 'Create Team Development AWS Accounts'
menuTitle: '7. Create Dev AWS Accounts'
disableToc: true
weight: 70
---

{{% comment %}}
Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: CC-BY-SA-4.0
{{% /comment %}}

In this step your Cloud Administrators will create several new team development AWS accounts via AWS Control Tower's Account Factory.

This step should take about 30 minutes to complete.

{{< toc >}}

## 1. Use at Least Two Team Development AWS Accounts from the Start

As highlighted previously, an AWS best practice is to isolate the work of distinct builder teams by assigning a different development AWS account to each team. Benefits of this approach include:

* **Inherent Per Team Cost Allocation:** Since AWS resource costs are, by default, attributable to each AWS account in which the resources are provisioned, at this early stage in your adoption, you don't need to force builder teams to use cost allocation tags on their resources.

* **Inherent Isolation Between Teams:** Since cloud resources managed by builder teams using different AWS accounts are, by default, completely isolated from each other, more advanced AWS Identity and Access Management (IAM) configurations are not needed to ensure that builder teams don't inadvertently impact each other's cloud resources.

Initially, you will likely need AWS accounts for the following teams:

|Team Development Account|Purpose|
|----------------|-------|
|**Workload Builder Team**|A team development AWS account for the team that will be doing the workload specific work for your first formal workload on AWS.|
|**Foundation Team**|A team development AWS account for the initial few Cloud and Security Administrators to experiment, develop, and perform early testing of changes to the foundation.|

## 2. Create Team Development AWS Accounts

In AWS Control Tower, provision the initial set of team development AWS accounts for early experimentation, development, and testing. 

You'll follow these steps twice: Once to create the initial development team's AWS account and again to create the development AWS account for the foundation team.

1. As a Cloud Administrator, use your personal user to log into AWS SSO.
2. Select the AWS **`master`** account.
3. Select **`Management console`** associated with the **`AWSAdministratorAccess`** role.
4. Select the appropriate AWS region.
5. Navigate to **`Control Tower`**.
6. Select **`Account Factory`** on the left.
7. In the upper right, click **`Enroll account`**.
8. Fill out the Enroll account form details. Some suggested fields are below:

|Field|Recommendation|
|-----|---------------|
|**`Account email`**|Consult the [set of AWS account root user email addresses]({{< relref "04-address-prerequisites.md#2-create-email-addresses-for-new-aws-accounts" >}}) that you established earlier.|
|**`Display name`**|**`team-a-dev`** or **`foundation-dev`**|
|**`AWS SSO email`**|Use the same email address as **`Account Email`**.|
|**`AWS SSO First Name`**|Use a part of your account name. For example, **`Team A`** or **`Foundation`** for the foundation team's development AWS account.|
|**`AWS SSO Last Name`**|Use the remaining part of the account name. For example, **`Development`**|
|**`Organizational unit`**|Select the development OU you created earlier in this section. For example, **`development`**.|

9. Select **`Enroll Account`**.

It will take a few minutes to enroll the new account. You can check the status in **`Service Catalog`**. Once it's done, the e-mail address you used will receive **3** messages: 1) **Your AWS Account is Ready**, 2) **Invitation to join AWS Single Sign-On**, 3) **Welcome to Amazon Web Services.**

## 3. Initialize AWS Account System Users

When each new team development AWS account is created, follow these steps to initialize the AWS account's AWS SSO user and root user to align with security best practices.

### Initialize AWS SSO User for the AWS Account
When a new AWS account has been created via the Account Factory, a user for the new AWS account is created in AWS SSO. As a best practice, you should initiatize the associated user's password and enable MFA. 

1. Access the inbox for the email address you associated with the AWS account when using Account Factory.
2. Within the email message "Invitation to join AWS Single Sign-On", select `Accept invitation`.
3. Follow the process to set the initial password for this user.

Follow the instruction in [How to Register a Device for Use with Multi-Factor Authentication](https://docs.aws.amazon.com/singlesignon/latest/userguide/user-device-registration.html).

### Initialize AWS Account's Root User

In addition to a new AWS SSO user being created for the AWS account, the new AWS account has a built-in root user.  

See [Log In as Root User](https://docs.aws.amazon.com/controltower/latest/userguide/best-practices.html#root-login) in the AWS Control Tower documentation for instructions to set the root user’s password.

See [Enable MFA on the AWS Account Root User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html#id_root-user_manage_mfa) for instructions to enable MFA.

## 4. Provide Cloud Administrators Access to New AWS Accounts

Since Cloud Administrators won't automatically be granted sufficient access to newly created AWS accounts, you need to enable this access each time you create new AWS accounts via AWS Control Tower's Account Factory.

1. As a Cloud Administrator, use your personal user to log into AWS SSO.
2. Select the AWS **`master`** account.
3. Select **`Management console`** associated with the **`AWSAdministratorAccess`** role.
4. Select the appropriate AWS region.
5. Navigate to **`AWS SSO`**.
6. Access **`AWS accounts`** in AWS SSO.
7. Select the checkboxes next both team development AWS accounts. For example:
  * `Team A - Dev`
  * `Foundation - Dev`
8. Select **`Assign users`**.
9. Select **`Groups`**.
10. Select the checkbox next to the group `example-cloud-admin` or similar.
11. Select **`Next: Permission sets`**.
12. Select the checkbox next to **`AWSAdministratorAccess`**.
13. Select **`Finish`**.

Now you've enabled all users who are part of the Cloud Administrator group in AWS SSO administrator access to the selected AWS accounts.

## 5. Apply Names to Shared Private Subnets

Since the names of shared subnets are not currently propagated to AWS accounts, as a Cloud Administrator, you should apply names to the shared subnets within each team development AWS account so that it's easier for the builder teams to understand the role of each subnet as they configure resources for AWS services.

1. As a Cloud Administrator, use your personal user to log into AWS SSO.
2. Select the team development AWS account of interest.
3. Select **`Management console`** associated with the **`AWSAdministratorAccess`** role.
4. Select the appropriate AWS region.
5. Navigate to **`VPC`**.
6. Select **`Your VPCs`**.
7. Select the unnamed VPC and assign the same name as used in the Network AWS account. For example, **`base-dev`**.
8. Select **`Subnets`**.
9. Update the **`Name`** field of each private subnet to match the name of the private subnet as it's configured in the `Network` AWS account. You can open another icognito or similar browser session to view the `Network` account's resources. **Caution:** The subnets may not be listed in the same order in both AWS accounts by default.
10. You can optionally apply the same naming alignment for route tables.