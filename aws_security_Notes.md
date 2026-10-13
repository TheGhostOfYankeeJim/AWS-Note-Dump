# SECURITY IN AWS

## Encyption 101

CLient side encyrption just means you the person encrypts the data before you send it to AWS. Nothing else ground shaking in this section. 

## AWS KMS

Two Types: 

AES-256 Symmetric Key 
You never get access to the KMS Key unencrypted

Asymmetric (RSA and ECC Key Pairs)
PKI Public Key == Encrypt, Private Key == Decrypt

Also Sign/Verify 
YOu can download the public key but not the private key 
This is usful for users outside of AWS that can't use the KMS API

## AWS KMS Keys 

AWS Owned Keys: 

Aws Managed Keys:
AWS/ServiceName
Automatic ever 1 year roll over

Customer Managed Keys:
Your external keys, $1 per month
Automatic and on demand key rotation

Imported KMS Key is only possible using manual 
$1 a month 
rotation using an alias 

KMS Keys are scope PER REGION

## KMS Key Policies
Default == anyone in your account has access to this key.

Anything that isn't the default is custom (Shocker):
Define who can admin the key. This is useful for cross account stuff. 

## Multi-region Keys 

Key ID stays the same in all regions

Encyrpt in one region decrypt in other region. 
Each Multi-Region key is managed independently 

Can encrypt part of a DynamoDB Gobal Table, you can encrypt parts of that data//fields so that even the Database Adminastrators can't decrypt. 

## S3 Replication Encryption

Any unencypted objects and encrypted are replicated (SSE-SE)
Objects encrypted with SSE-C (Customer Keys) can be replicated

For object encyrpted with SSE-KMS you need to enable the replication options
    - This will require you stating which KMS key to encrypted the objects to the target bucket. 
    Change the Key POlicy to allow this
    IAM Role with KMS:Decrypt for the source KMS Key and kms:Encrypt for the target KMS key 

## Encryption AMI with KMS 

MOdify the image attribute with a launch permission with the target account

Share the keys via a key policy 

The target role/userin the target account needs: DescribeKey, ReEncrypt*, CreateGrant, and decrypt. 

## SSM Parameter Store
Store encrypted configs for your applications 

This might be worth building a scanner looking for secure strings. 

## AWS Secrets Manager

Capability to force the rotation of secrets every X days

Auto generation of secrets on rotation (LAMBDA)
Integration with Amazon RDS (MySQL, POstGres,Aurora)
Secrets are Encrypted using KMS

Mostly for RDS

Multi-region Secrets
Rep across multi-regions, red replicas in sync with primary Secret. 

## AWS Certificate Manager (ACM)
Manage and deploy TLS Certs
Provides websites flight encryption (HTTPS)

Supports both public and private TLS certs

Free of charge for public TLS certs

Auto renewal

Integrations with ELV (CLB, ALB, NLB)
CloudFront Distributions

## CloudHSM

Dedicated Hardware for encyrption but we manage the encryption keys. 

Has FIPS 1 40-2 level 3 complaince
can do symmetric and asymmetric encryptions

CRUD as an HSM CLuster

CloudHSM Software actually manages the keys. 

MultiAZ replicate one vault to another AZ. 

Can connect this to AWS services by CloudHSM connector to AWS KMS, API calls made through API are logged in CloudTrail.

## AWS WAF

Works on level 7 HTTP
Deploy it on ALB, Api Gateway, CLoudFront, AppSync GraphQL API, Cognito User Pool 

Web appliation firewall can't bbe deployed on a NLB (duh)

Can define ACLs, like up too 10000 IP addresses per rule
Filter on headers, bodys, URI strings, ext. 
Geo-match countries 
Rate-based rules 

## AWS Sheild

DDOS protection, 

Standard Sheild, syn/udp floods, reflection attacks, other layer 3/4 everyone has this one. 

AWS Advance, $3000 per month per org
DDOS response team, Protect against higher usage fees
Ec2, ELB, CloudFront, Gobal Accelerator, and route53 attacks

## AWS Firewall Managers
Manage all rules in all accounts of an AWS Org

Sec Policy: Common Set of Rules 

## Best Practicces - DDOS 

BP1 - CloudFront - uses shield
BP1 - Global Accelorator - uses shield 

BP3 - Route53

## GuardDuty 
Machine Learning to detect abnormal hits
Looks at CloudTrail Management Events, Event Logs, S3 Data Events, VPC Flow Logs, DNS Logs(Encoded data sent)
Can protect you against CryptoCurrency Attacks

can use event bridge to trigger lambda or SNS

## Amazon Inspector
Automated Security Assesments

EC2, SSM Agent, looks for unintended network accessibility, looks at OS for vulns. 

So this service only works with EC2, Container Images, and Lambda Functions - Finds CVEs, maps network reachabiity EC2

## Macie 

Data Security and data privacy service, alerts you if you have PII in a S3 Bucket. (Your blog post covers this)

22/25 correct. 