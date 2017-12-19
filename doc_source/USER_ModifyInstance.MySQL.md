# Modifying a DB Instance Running the MySQL Database Engine<a name="USER_ModifyInstance.MySQL"></a>

You can change the settings of a DB instance to accomplish tasks such as adding additional storage or changing the DB instance class\. This topic guides you through modifying an Amazon RDS MySQL DB instance, and describes the settings for MySQL instances\. 

We recommend that you test any changes on a test instance before modifying a production instance, so that you fully understand the impact of each change\. This is especially important when upgrading database versions\. 

After you modify your DB instance settings, you can apply the changes immediately, or apply them during the next maintenance window for the DB instance\. Some modifications cause an interruption by restarting the DB instance\. 

## AWS Management Console<a name="USER_ModifyInstance.MySQL.Console"></a>

**To modify a MySQL DB instance**

1. Sign in to the AWS Management Console and open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **DB Instances**, and then select the DB instance that you want to modify\. 

1. Choose **Instance Actions**, and then choose **Modify**\. The **Modify DB Instance** page appears\.

1. Change any of the settings that you want\. For information about each setting, see [Settings for MySQL DB Instances](#USER_ModifyInstance.MySQL.Settings)\. 

1. To apply the changes immediately, select **Apply Immediately**\. Selecting this option can cause an outage in some cases\. For more information, see [The Impact of Apply Immediately](Overview.DBInstance.Modifying.md#USER_ModifyInstance.ApplyImmediately)\. 

1. When all the changes are as you want them, choose **Continue**\. 

1. On the confirmation page, review your changes\. If they are correct, choose **Modify DB Instance** to save your changes\. 

   Alternatively, choose **Back** to edit your changes, or choose **Cancel** to cancel your changes\. 

## CLI<a name="USER_ModifyInstance.MySQL.CLI"></a>

To modify a MySQL DB instance by using the AWS CLI, call the [modify\-db\-instance](http://docs.aws.amazon.com/cli/latest/reference/rds/modify-db-instance.html) command\. Specify the DB instance identifier, and the parameters for the settings that you want to modify\. For information about each parameter, see [Settings for MySQL DB Instances](#USER_ModifyInstance.MySQL.Settings)\. 

**Example**  
The following code modifies `mydbinstance` by setting the backup retention period to 1 week \(7 days\)\. The code disables automatic minor version upgrades by using `--no-auto-minor-version-upgrade`\. To allow automatic minor version upgrades, use `--auto-minor-version-upgrade`\. The changes are applied during the next maintenance window by using `--no-apply-immediately`\. Use `--apply-immediately` to apply the changes immediately\. For more information, see [The Impact of Apply Immediately](Overview.DBInstance.Modifying.md#USER_ModifyInstance.ApplyImmediately)\.   
For Linux, OS X, or Unix:  

```
aws rds modify-db-instance \
    --db-instance-identifier mydbinstance \
    --backup-retention-period 7 \
    --no-auto-minor-version-upgrade \
    --no-apply-immediately
```
For Windows:  

```
aws rds modify-db-instance ^
    --db-instance-identifier mydbinstance ^
    --backup-retention-period 7 ^
    --no-auto-minor-version-upgrade ^
    --no-apply-immediately
```

## API<a name="USER_ModifyInstance.MySQL.API"></a>

To modify a MySQL instance by using the Amazon RDS API, call the [ModifyDBInstance](http://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_ModifyDBInstance.html) action\. Specify the DB instance identifier, and the parameters for the settings that you want to modify\. For information about each parameter, see [Settings for MySQL DB Instances](#USER_ModifyInstance.MySQL.Settings)\. 

**Example**  
The following code modifies `mydbinstance` by setting the backup retention period to 1 week \(7 days\) and disabling automatic minor version upgrades\. These changes are applied during the next maintenance window\.   

```
 1. https://rds.amazonaws.com/
 2.     ?Action=ModifyDBInstance
 3.     &ApplyImmediately=false
 4.     &AutoMinorVersionUpgrade=false
 5.     &BackupRetentionPeriod=7
 6.     &DBInstanceIdentifier=mydbinstance
 7.     &SignatureMethod=HmacSHA256
 8.     &SignatureVersion=4
 9.     &Version=2014-10-31
10.     &X-Amz-Algorithm=AWS4-HMAC-SHA256
11.     &X-Amz-Credential=AKIADQKE4SARGYLE/20131016/us-west-1/rds/aws4_request
12.     &X-Amz-Date=20131016T233051Z
13.     &X-Amz-SignedHeaders=content-type;host;user-agent;x-amz-content-sha256;x-amz-date
14.     &X-Amz-Signature=087a8eb41cb1ab0fc9ec1575f23e73757ffc6a1e42d7d2b30b9cc0be988cff97
```

## Settings for MySQL DB Instances<a name="USER_ModifyInstance.MySQL.Settings"></a>

The following table contains details about which settings you can modify, which settings you can't modify, when the changes can be applied, and whether the changes cause downtime for the DB instance\. 


****  

| Setting | Setting Description | When the Change Occurs | Downtime Notes | 
| --- | --- | --- | --- | 
|  Allocated Storage  |  The storage, in gigabytes, that you want to allocate for your DB instance\.  For more information, see [Storage for Amazon RDS](CHAP_Storage.md)\.   |  If **Apply Immediately** is set to true, the change occurs immediately\.  If **Apply Immediately** is set to false, the change occurs during the next maintenance window\.   |  No downtime\. Performance may be degraded during the change\.   | 
|  Auto Minor Version Upgrade  |  **Yes** if you want your DB instance to receive minor engine version upgrades automatically when they become available\. Upgrades are installed only during your scheduled maintenance window\.   | – | – | 
|  Backup Retention Period  |  The number of days that automatic backups are retained\. To disable automatic backups, set the backup retention period to 0\.  For more information, see [Working With Backups](USER_WorkingWithAutomatedBackups.md)\.   |  If **Apply Immediately** is set to true, the change occurs immediately\.  If **Apply Immediately** is set to false and you change the setting from a non\-zero value to another non\-zero value, the change is applied asynchronously, as soon as possible\. Otherwise, the change occurs during the next maintenance window\.   |  An outage occurs if you change from 0 to a non\-zero value, or from a non\-zero value to 0\.   | 
|  Backup Window  |  The time range during which automated backups of your databases occur\. The backup window is a start time in Universal Coordinated Time \(UTC\), and a duration in hours\.  For more information, see [Working With Backups](USER_WorkingWithAutomatedBackups.md)\.   |  The change is applied asynchronously, as soon as possible\.   | – | 
|  Certificate Authority  |  The certificate that you want to use\.   | – | – | 
|  Copy Tags to Snapshots  |  If you have any DB instance tags, this option copies them when you create a DB snapshot\.  For more information, see [Tagging Amazon RDS Resources](USER_Tagging.md)\.   | – | – | 
|  Database Port  |  The port that you want to use to access the database\.  The port value must not match any of the port values specified for options in the option group for the DB instance\.   |  The change occurs immediately\. This setting ignores the **Apply Immediately** setting\.   |  The DB instance is rebooted immediately\.  | 
|  DB Engine Version  |  The version of the MySQL database engine that you want to use\. Before you upgrade your production DB instances, we recommend that you test the upgrade process on a test instance to verify its duration and to validate your applications\.  For more information, see [Upgrading the MySQL DB Engine](USER_UpgradeDBInstance.MySQL.md)\.   |  If **Apply Immediately** is set to true, the change occurs immediately\.  If **Apply Immediately** is set to false, the change occurs during the next maintenance window\.   |  An outage occurs during this change\.  | 
|  DB Instance Class  |  The DB instance class that you want to use\.  For more information, see [DB Instance Class](Concepts.DBInstanceClass.md)\.   |  If **Apply Immediately** is set to true, the change occurs immediately\.  If **Apply Immediately** is set to false, the change occurs during the next maintenance window\.   |  An outage occurs during this change\.  | 
|  DB Instance Identifier  |  The DB instance identifier\. This value is stored as a lowercase string\.  For more information about the effects of renaming a DB instance, see [Renaming a DB Instance](USER_RenameInstance.md)\.   |  If **Apply Immediately** is set to true, the change occurs immediately\.  If **Apply Immediately** is set to false, the change occurs during the next maintenance window\.   |  An outage occurs during this change\. The DB instance is rebooted\.   | 
|  DB Parameter Group  |  The parameter group that you want associated with the DB instance\.  For more information, see [Working with DB Parameter Groups](USER_WorkingWithParamGroups.md)\.   |  The parameter group change occurs immediately\. However, parameter changes only occur when you reboot the DB instance manually without failover\.  For more information, see [Rebooting a DB Instance](USER_RebootInstance.md)\.   |  An outage doesn't occur during this change\. However, parameter changes only occur when you reboot the DB instance manually without failover\.   | 
|  Enable Enhanced Monitoring  |  **Yes** to enable gathering metrics in real time for the operating system that your DB instance runs on\.  For more information, see [Enhanced Monitoring](USER_Monitoring.OS.md)\.   | – | – | 
|  Enable IAM DB Authentication  |  **Yes** to enable IAM database authentication for this DB instance\.  For more information, see [IAM Database Authentication for MySQL and Amazon Aurora](UsingWithRDS.IAMDBAuth.md)\.   | – | – | 
|  Maintenance Window  |  The time range during which system maintenance occurs\. System maintenance includes upgrades, if applicable\. The maintenance window is a start time in Universal Coordinated Time \(UTC\), and a duration in hours\.  If you set the window to the current time, there must be at least 30 minutes between the current time and end of the window to ensure any pending changes are applied\.  For more information, see [The Amazon RDS Maintenance Window](USER_UpgradeDBInstance.Maintenance.md#Concepts.DBMaintenance)\.   |  The change occurs immediately\. This setting ignores the **Apply Immediately** setting\.   |  If there are one or more pending actions that cause an outage, and the maintenance window is changed to include the current time, then those pending actions are applied immediately, and an outage occurs\.   | 
|  Multi\-AZ Deployment  |  **Yes** to deploy your DB instance in multiple Availability Zones; otherwise, **No**\.  For more information, see [Regions and Availability Zones](Concepts.RegionsAndAvailabilityZones.md)\.   |  If **Apply Immediately** is set to true, the change occurs immediately\.  If **Apply Immediately** is set to false, the change occurs during the next maintenance window\.   |  –  | 
|  New Master Password  |  The password for your master user\. The password must contain from 8 to 41 alphanumeric characters\.   |  The change is applied asynchronously, as soon as possible\. This setting ignores the **Apply Immediately** setting\.   |  –  | 
|  Option Group  |  The option group that you want associated with the DB instance\.  For more information, see [Working with Option Groups](USER_WorkingWithOptionGroups.md)\.   |  If **Apply Immediately** is set to true, the change occurs immediately\.  If **Apply Immediately** is set to false, the change occurs during the next maintenance window\.   |  –  | 
|  Publicly Accessible  |  **Yes** to give the DB instance a public IP address, meaning that it is accessible outside the VPC\. To be publicly accessible, the DB instance also has to be in a public subnet in the VPC\. **No** to make the DB instance accessible only from inside the VPC\.  For more information, see [Hiding a DB Instance in a VPC from the Internet](USER_VPC.WorkingWithRDSInstanceinaVPC.md#USER_VPC.Hiding)\.   |  The change occurs immediately\. This setting ignores the **Apply Immediately** setting\.   |  –  | 
|  Security Group  |  The security group you want associated with the DB instance\.  For more information, see [Working with DB Security Groups \(EC2\-Classic Platform\)](USER_WorkingWithSecurityGroups.md)\.   |  The change is applied asynchronously, as soon as possible\. This setting ignores the **Apply Immediately** setting\.   |  –  | 
|  Storage Type  |  The storage type that you want to use\.  For more information, see [Amazon RDS Storage Types](CHAP_Storage.md#Concepts.Storage)\.   |  If **Apply Immediately** is set to true, the change occurs immediately\.  If **Apply Immediately** is set to false, the change occurs during the next maintenance window\.   |  The following changes all result in a brief outage while the process starts\. After that, you can use your database normally while the change takes place\.  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ModifyInstance.MySQL.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ModifyInstance.MySQL.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ModifyInstance.MySQL.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ModifyInstance.MySQL.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ModifyInstance.MySQL.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ModifyInstance.MySQL.html)  | 
|  Subnet Group  |  The subnet group for the DB instance\. You can use this setting to move your DB instance to a different VPC\.  If your DB instance is not in a VPC, you can use this setting to move your DB instance into a VPC\.  For more information, see [Moving a DB Instance Not in a VPC into a VPC](USER_VPC.WorkingWithRDSInstanceinaVPC.md#USER_VPC.Non-VPC2VPC)\.   | – | – | 

## Related Topics<a name="USER_ModifyInstance.MySQL.Related"></a>

+ [Rebooting a DB Instance](USER_RebootInstance.md)

+ [Connecting to a DB Instance Running the MySQL Database Engine](USER_ConnectToInstance.md)

+ [Upgrading the MySQL DB Engine](USER_UpgradeDBInstance.MySQL.md)

+ [Deleting a DB Instance](USER_DeleteInstance.md)