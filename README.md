# 1. Parameter Groups

> Parameter groups are used to configure the databases we utilize, and they differ from one database to the next as separate sets of parameters. This is comparable to setting up “my.cnf file” 

> Parameters can be divided into two categories:
> - Static (modifications take effect after a manual reboot/maintenance window)
> - Dynamic ( changes apply immediately)

> You manage your database configuration by associating your DB instances and Multi-AZ DB clusters with parameter groups. Amazon RDS defines parameter groups with default settings. You can also define your own parameter groups with customized settings.

> Some DB engines offer additional features that you can add to your database as options in an option group.

> A DB parameter group acts as a container for engine configuration values that are applied to one or more DB instances

> DB cluster parameter groups apply to Multi-AZ DB clusters only. In a Multi-AZ DB cluster, the settings in the DB cluster parameter group apply to all of the DB instances in the cluster. The default DB parameter group for the DB engine and DB engine version is used for each DB instance in the DB cluster.

> You can't modify the parameter settings of a default parameter group.

> Not all DB engine parameters in a parameter group are eligible to be modified.


# 2. Bash Script to Restore

https://github.com/becatini/rds-snapshot-restore/blob/main/rds-restore.sh

- After the restore done, compare if the original RDS instance has only one **VPC Security Group**.
- **Tags** will be copied from snapshot.
- Make sure you modify **Parameter Group** after restore is completed.
- No multi-AZ. 
- No public access.

> Our OutlineDB as of now is in `no-multi-az` and `is-publicly-accessible`
# 3. RDS Publicly Accessible

One common Amazon RDS scenario is to have a VPC in which you have an EC2 instance with a public-facing web application and a DB instance with a database that isn't publicly accessible. For example, you can create a VPC that has a public subnet and a private subnet. Amazon EC2 instances that function as web servers can be deployed in the public subnet. The DB instances are deployed in the private subnet. In such a deployment, only the web servers have access to the DB instances. For an illustration of this scenario, see [A DB instance in a VPC accessed by an EC2 instance in the same VPC](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.Scenarios.html#USER_VPC.Scenario1).

When you launch a DB instance inside a VPC, the DB instance has a private IP address for traffic inside the VPC. This private IP address isn't publicly accessible. You can use the **Public access** option to designate whether the DB instance also has a public IP address in addition to the private IP address. If the DB instance is designated as publicly accessible, its DNS endpoint resolves to the private IP address from within the VPC. It resolves to the public IP address from outside of the VPC. Access to the DB instance is ultimately controlled by the security group it uses. That public access is not permitted if the security group assigned to the DB instance doesn't include inbound rules that permit it. In addition, for a DB instance to be publicly accessible, the subnets in its DB subnet group must have an internet gateway. For more information, see [Can't connect to Amazon RDS DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Troubleshooting.html#CHAP_Troubleshooting.Connecting)

![[image1.png]]