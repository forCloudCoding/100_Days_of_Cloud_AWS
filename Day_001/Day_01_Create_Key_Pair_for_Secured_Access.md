
# Day 01 : Create a Key Pair for Secured Access to EC2



## 🎯 Task :

Create a Key Pair with the following requirements :

Key Pair Name : `devops-kp`\
Key Pair Type : `rsa`

#### AWS Menu Navigation :
AWS Console [ Region : N. Virginia ] --> EC2 --> Network & Security --> Key Pairs

## 🌍 Real-World Scenario :
Losing an AWS EC2 private key pair (.pem file) is a common operational mishap in real-world IT and DevOps environments that temporarily halts SSH access

#### Modern Alternatives : 
AWS Systems Manager (SSM) SSM Agent is pre-installed with correct IAM permissions, administrators can bypass SSH keys entirely and run automation documents like AWSSupport-ResetAccess to regain entry instantly.

## ⚠️ Security Compliance Violation : 
Launching a brand-new key without cleaning up old keys in `~/.ssh/authorized_keys`, will create hidden entry points that violate Security Compliance.

## 📚 What is an AWS EC2 Key Pair ?

An Amazon EC2 key pair is a set of security credentials used to prove our identity when connecting to an AWS EC2 instance. It relies on public-key cryptography to eliminate the need for traditional usernames and passwords, providing a highly secure method for remote login.

