
# Day 05 : Create GP3 Volume


## 🎯 Task :

Create an EBS Volume with the following requirements:\
**Volume Type :** ```gp3```\
**Volume Size :** ```2 GiB```

#### AWS Menu Navigation :
AWS Console [ Region : N. Virginia ] --> EC2 --> EBS --> Volumes

## 🌍 Real-World Scenario : 

#### Running Out of Disk Space on a Live Volume

* The Problem : A production database or application server fills up its EBS volume, risking crashes or data write failures.
* The Solution :
	1. Modify the EBS volume size directly from the AWS Management Console or AWS CLI (no need to stop the EC2 instance).
	2. Connect to the EC2 instance via SSH and run OS-level filesystem expansion commands (such as growpart and resize2fs for ext4, or xfs_growfs for XFS).

#### Disaster Recovery and Cross-Region Migration

* **The Problem :** An entire AWS Availability Zone or Region experiences an outage, or you need to migrate an application stack to a new geographical location.
* **The Solution :**
	1. Take a point-in-time EBS Snapshot of the volume.
	2. Copy the snapshot to the target AWS Region.
	3. Create a new EBS volume from that copied snapshot in the destination AZ and attach it to your recovery EC2 instance.

#### Cost Optimization by Switching Volume Types

* **The Problem :** Over-provisioned high-performance storage (like io2) is driving up monthly cloud bills for workloads that have predictable or low-demand periods.
* **The Solution :** Use EBS Elastic Volumes to dynamically modify and downgrade or upgrade volume types (e.g., shifting from st1 HDD to gp3 SSD during peak traffic hours, then back down) without detaching the drive or facing downtime.

## ✅ AWS EBS Best Practices :

**Default to gp3 :** Use gp3 General Purpose SSD volumes as your default choice for most workloads to balance cost and performance.
**Match Workload to type :** Reserve io1 or io2 (including io2 Block Express) for mission-critical databases, and use st1 or sc1 for large sequential throughput or cold storage.
**Automate Back-Ups :** Use AWS Backup or automated snapshot life-cycle policies to create incremental backups of critical volumes.
**Enable Recycle Bin :** Configure the EBS Snapshot Recycle Bin with retention periods (from 1 day up to 1 year) to guard against accidental deletions.
**Clean-Up unused Assets :** Regularly delete detached or empty volumes and old, unneeded snapshots.
**Enable Encryption by default :** Turn on default encryption per-region in your EC2 Console Settings so all new volumes and snapshot copies are automatically encrypted using AWS KMS.


## 📚 What is an EBS ?

**AWS EBS** allows us to create storage volumes and attach them to **EC2 Instances**. Once attached, we can create a file system on top of these volumes, run a database, or use them just like we would use block storage. EBS volumes are placed in a **specific AZs**, where they are automatically replicated to protect from the failure of a single component. All EBS volume types offer durable snapshot capabilities and are designed for high availability.


#### Quick Comparison of Popular EBS Types

* **General Purpose (gp3 / gp2) :** Balanced price and performance for mainstream applications and boot volumes.
* **Provisioned IOPS (io2 Block Express / io1) :** High-performance SSDs built for mission-critical, low-latency relational or NoSQL databases.
* **Throughput Optimized / Cold HDD (st1 / sc1) :** Low-cost magnetic storage for large, sequential streaming and big data workloads.

