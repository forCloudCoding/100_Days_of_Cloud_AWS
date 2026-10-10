
# Day 04 : Enable Versioning for S3 Bucket


## 🎯 Task :
Create a S3 Bucket with below details :
Name : ```devops-s3-31538006```
Versioning : ```Enabled```

#### AWS Menu Navigation :
AWS Console [ Region : N. Virginia ] --> S3 --> General Purpose Buckets --> Create Bucket

## 🌍 Real-World Scenario : 

A DevOps engineer manages a critical production-config.json file inside an S3 bucket ( Versioning Enabled ) used by a live web service.


#### The Accident

* An engineer runs a deployment script that uploads a broken production-config.json (with a typo in the database connection string) to the exact same key name.
* In an unversioned bucket, the old configuration would be permanently overwritten and lost.
* Because versioning is enabled, S3 assigns a unique Version ID to the new upload and keeps the previous file intact as a noncurrent version.

#### The Discovery of Accident & Recovery of Correct version of Config File

* The application crashes because of the bad config.
* The engineer navigates to the S3 bucket's Versions tab in the console (or uses the AWS CLI).
* The Engineer see two versions of production-config.json :
	* **Current Version :** The broken file with a new Version ID.
	* **Previous Version :** The working file with its original Version ID.
* To fix it, the Engineer has two choices :
	* **Option A (Delete current) :** Permanently delete the broken current version. S3 automatically promotes the older working version back to the current version.
	* **Option B (Restore/Re-upload) :** Download the older version or copy it over as a new current version.



## ✅ AWS S3 Best Practices :


#### 🔒 Security & Access Control

* **Block Public Access (Always) :** Enable S3 Block Public Access at both the bucket and account levels. This serves as a centralized safeguard against misconfigured bucket policies.
* **Disable Access Control Lists (ACLs) :** Enforce the Bucket Owner Enforced setting to completely disable legacy ACLs. This ensures that all access is managed consistently through unified AWS Identity and Access Management (IAM) and bucket policies.
* **Enforce Least Privilege :** Never use wildcard permissions ("Action": "s3:*" or "Resource":"*") in production environments. Grant applications micro-targeted roles that restrict access to specific buckets or naming prefixes.
* **Secure Data in Transit :** Create a bucket policy that explicitly denies any requests that do not utilize HTTPS/TLS (using the aws :SecureTransport condition).

#### 📉 Cost Optimization & Lifecycle Management

* **Implement Lifecycle Rules :** Automatically transition aging data to colder storage classes (e.g., S3 Standard-IA, S3 Glacier, or Glacier Deep Archive) to save on storage costs.
* **Clean Up Incomplete Multipart Uploads :** Configure a lifecycle rule to automatically abort and remove uncompleted multipart uploads after a specified number of days to prevent hidden storage costs.
* **Leverage Intelligent-Tiering :** For dynamic or unpredictable data access patterns, use the S3 Intelligent-Tiering storage class to automate cost savings without operational overhead.

#### 🛡️ Data Protection & Resiliency

* **Enable Versioning :** Protect against accidental deletion or malicious overwrites by enabling S3 Versioning. If an object is deleted, it can be easily restored to a prior version.
* **Turn on MFA Delete :** Combine versioning with Multi-Factor Authentication (MFA) Delete. This forces secondary verification before permanently purging an object version or change the bucket's versioning status.
* **Use Cross-Region Replication (CRR) :** For mission-critical disaster recovery plans, replicate your data to a bucket in a physically separate AWS region.

#### ⚡ Performance Optimization

* **Design Scalable Prefixes :** S3 scales horizontally and handles at least 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix. Distribute highly active files across unique virtual folder prefixes to avoid bottlenecks.
* **Integrate Amazon CloudFront :** For content heavy applications distribution, route requests through Amazon CloudFront. It caches data at global edge locations, speeding up retrieval while restricting direct access to your origin S3 bucket.
* **Use S3 Bucket Keys with KMS :** When employing server-side encryption via AWS KMS (SSE-KMS), enable S3 Bucket Keys. This drastically reduces KMS request traffic and lowers the encryption call costs by up to 99%.

#### 👁️ Monitoring & Auditing

* **Enable Server Access Logs :** Keep detailed logs of all requests sent to the bucket for security compliance. Store these access logs in a completely separate, dedicated log bucket.
* **Deploy Automated Security Tools :** Turn on Amazon GuardDuty for continuous threat monitoring and use Amazon Macie to scan for exposed Personal Identifiable Information (PII) or sensitive data in the objects.



## 📚 What is an S3 Bucket ?

An Amazon S3 bucket is a public cloud storage container used to store files and data within Amazon Web Services (AWS) Simple Storage Service (S3).

#### Key Characteristics of S3 Bucket :

* **Global Namespace :** Every bucket name must be completely unique across all AWS accounts worldwide, similar to a website domain name.
* **Region-Specific :** When creating a bucket, we assign it to a specific physical geographic location (e.g., US East, EU West). The data stays in that region unless we explicitly move or replicate it.
* **Virtually Unlimited Storage :** While individual objects can range in size from 0 bytes up to 5 Terabytes, a single bucket can hold an infinite amount of data.
* **Private by Default :** All buckets are completely secure and private when created. We must explicitly configure access rules using AWS IAM or custom bucket policies to share data.

#### Main Types of S3 Buckets :

AWS S3 supports four main types of Buckets, each built for specific workloads and data access patterns :

1. **General Purpose Buckets :** These are the standard, default bucket type used for the vast majority of AWS workloads.
2. **Directory Buckets :** These are purpose-built for extreme high-performance and low-latency applications.
3. **Table Buckets :** Designed specifically for storing structured tabular data without the overhead of maintaining objects.
4. **Vector Buckets :** Built to support AI and generative machine learning ecosystems.

