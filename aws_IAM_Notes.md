## General Knowledge
IAM --> Idenity and Access Management
Its one of the global services
All Accounts start with a Root account (important account like root on linux distros)
    - ideally its recommended that this account is not used or shared as a production account
    - You use this account to create new users, this is to get you started

A lot of the "old" wisdom from managing AD applies here:
    - You can break users up into groups (i.e. developers vs operations)
    HOWEVER: groups CAN ONLY contain users, not other groups
    This way you don't run into nested permission issues like you do in AD

Users don't have to belong to group, and be part of multiple groups. 

Users and groups can be given "policies" (its a json file listing what they can and can't do)
    - This is like how you can assign a GPO to a AD group. 

Just like AD, you want to use the least privilege principal. 
    - i.e. in AD the most dangerous users are the ones that have moved around the company a bunch but never returned any of their hats, domain admin everything but in name situation. 

Only give the bare min for users to do their job. 

## Creating Users and IAM policies. 

Any users you create here will be gobally accessible because IAM is a global service (duh)

When making a new users use the "I want to create an IAM user" options. 
Autogenerate user password, make user change their password on next login. 

Tags are essentially just bits of metadate to help with searching and sorting in large environments. 

Creating an Alias for the account can change their logon URL to something more human friendly and they don't have to use their account number on the IAM Account ID field. 

You can use two accounts at the same time with AWS Simultaneous Sign-in now. 

## IAM Inheritance 
Inline policy == a AWS policy attached to a user directly.
Otherwise whatever polcies that are defined onto the groups will get passed onto the group memers
If a user belongs to multiple groups they will take on the policies of all their groups. 
    - What happens is policies clash?

IAM Polcies Sturcture

SID == Its an ID for the statement (optional)
EFFECT == Allows or denies the access
Principal == user/account/role the policy is applied on (uses ARN names)
Action == List of actions such as pulling S3 buckets (S3:GetObject, S3.PutObject)
Resource == a list of resources which the actions can be used on (uses ARN names, like specific S3 Buckets)
Condition == When the policy starts its effect (i.e. after a merger I might want to deny access to certain users and resources after the merger is complete) This is optional as well. 

If you see something like 

```"Action": "*"```

or 

``` "Resource": "*"```

Its effectively a wild card giving everything. Like all actions allowed or all resources allowed. Becareful when using this. 

## IAM Password Policy and MFA
Can set minimum password length
Can set complexity requirements (Needs uppercase, numbers, special chars, etc)
Can set to have users change their passwords
Can set users to change their passwords after x time. 
Can stop password reuse
(this is like AD)

## IAM Roles for Services
Like a scanning service in AD. 
IAM ROLE is like a user but used by AWS services. 
IAM ROLE == my Nessus scanning service. 
Common roles: 
    - EC2 Roles
    - Lambda Fuctions
    - CloudFormation

## IAM Sec Tools
IAM Cred Report
    - lists all your accounts users and status of their creds
This is like pulling AD member fields (Last password set, when forced reset, etc)


# IAM Access Advisor
This is a good oppurtunity to see what service permissions are attached to which users
When they were last used
This is a good method for apply the principale of least privilege. 

# IAM Best Practices
Try to not use the Root account, use newly created accounts with least privilege in mind
One IRL user = one AWS user
Assign permissions to groups, then add users to groups. 
USE MFA, use it!
Create roles for giving permissions to AWS services. 
Get into a habit of auditing your IAM Creds and Access Advisor
NEVER SHARE AWS ACCESS KEYS

# Multifactor
At a bare min, your ROOT account should have MFA, ideally all of your users should too.

MFA Devices:
Virtual MFA (Google Auth//Authy)
Universal 2nd Factor (U2F) Security Key
    - Yubikey (The one and only true MFA lol)
    - its usually a physical key

Hardware Key Fob Device (RSA keyfobs that generate the code on the device)

AWS GovCloud (key fob as well ((Surpass)))

## AWS Access Keys, CLI, SDK 

Three ways to access. 

First is the AWS Management console: password//MFA protected
CLI - Access Key protected
SDK (Like boto3, apis, etc) - protected by access keys
    - https://github.com/aws/aws-sdk-net

Users manage their own Access keys, treat like a password don't share them. 

When you create an access key you only get one oppurtunity to save the creds. 

Once you have your creds:

```aws configure ```

It'll ask for the cred info at this point. Then your ready to go. 

## Cloudshell
Its like the Azure CLI in the web browser. 
Not in every region though. But all this is the terminal via browser.
Its the same thing I used to set up NPK for cracking. 
(uses the user your currently logged in as)

The neat thing is all the files you create with your cloudshell environment will continue to exist. Even after closing the browswer, losing your session, etc. 

Can upload and download files, might be able to use this as a junky C2 channel. 

## Note to Self

Take some time to set up the Service Role for EC2

Review what is used inside an IAM Policy, I can't ever remember what specifically can be found in an IAM policy. 