# AWS RDS Terraform module
![squareops_avatar]

[squareops_avatar]: https://squareops.com/wp-content/uploads/2022/12/squareops-logo.png

### [SquareOps Technologies](https://squareops.com/) Provide end to end solution for all your DevOps needs
<br>
The terraform-aws-rds-mysql module is a reusable infrastructure-as-code solution for deploying and managing an Amazon RDS MySQL database cluster using Terraform. It simplifies the process of provisioning and configuring a highly available and scalable MySQL database environment in AWS.
Features

  1. High Availability: The module sets up a multi-AZ (Availability Zone) database cluster for enhanced fault tolerance and automatic failover.
  2. Scalability: Easily scale your database cluster by adjusting the instance count and instance type according to your needs.
  3. Security: The module integrates with AWS Identity and Access Management (IAM) for secure authentication and fine-grained access control.
  4. Backup and Recovery: Automated backups can be scheduled, and the module provides options for specifying the retention period and whether to skip a final snapshot during deletion.
  5. Encryption: Database encryption at rest can be enabled to ensure data security.
  6. Maintenance Window: Configure a maintenance window for performing regular database maintenance tasks.
  7. Public Accessibility: Choose whether the database cluster should be publicly accessible over the internet.
  8. Replication: Replicate data from another Amazon RDS database by specifying the source database identifier.
    Snapshot Restore: Restore the database from a specified snapshot ID to easily recreate database instances.
  9. VPC Support: Deploy the RDS cluster in a specific Virtual Private Cloud (VPC) and specify the associated subnets for network isolation.

## Usage Example

```hcl
module "rds-mysql" {
  source                  = "https://github.com/sq-ia/terraform-aws-rds-mysql.git"
  vpc_id                  = "vpc-0d2c255df1f"
  subnet_ids              = ["subnet-04cecf2400","subnet-0ac69f821"]
  family                  = "mysql8.0
  db_name                 = "proddb"
  multi_az                = false
  environment             = "prod"
  kms_key_arn             = "arn:aws:kms:us-east-2:2222222222:key/a22ecc12-4-ae1be7590774"
  engine_version          ="8.0.32"
  instance_class          = "db.t3.medium"
  master_username         = "admin"
  allocated_storage       = 20
  rds_instance_name       = "mysql"
  major_engine_version    = "8.0"
  allowed_security_groups = ["sg-0e2f946c67"]
  publicly_accessible     = false
  skip_final_snapshot              = true
  backup_window                    = "03:00-06:00"
  snapshot_identifier              = null
  maintenance_window               = "Mon:00:00-Mon:03:00"
  final_snapshot_identifier_prefix = "prod-snapshot"
  deletion_protection              = true
}

```
Refer [examples](https://gitlab.com/squareops/sal/terraform/aws/rds-mysql/-/tree/qa/examples/complete-mysql) directory for more references.


## Important Note
1. By default, the variable `create_random_password` is set to true. Therefore, even if the user provides a password, it will not be read. The `create_random_password` variable should be set to false and the `password` variable should have a non-null value to be read and used.

## Security & Compliance [<img src="	https://prowler.pro/wp-content/themes/prowler-pro/assets/img/logo.svg" width="250" align="right" />](https://prowler.pro/)

Security scanning is graciously provided by Prowler. Proowler is the leading fully hosted, cloud-native solution providing continuous cluster security and compliance.

| Benchmark | Description |
|--------|---------------|
| Ensure that encryption is enabled for RDS instances | Enabled for RDS created using this module. |
<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.0 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 4.23 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | >= 4.23 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_db"></a> [db](#module\_db) | terraform-aws-modules/rds/aws | 5.1.0 |
| <a name="module_security_group_rds"></a> [security\_group\_rds](#module\_security\_group\_rds) | terraform-aws-modules/security-group/aws | 4.13.0 |

## Resources

| Name | Type |
|------|------|
| [aws_secretsmanager_secret.secret_master_db](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/secretsmanager_secret) | resource |
| [aws_security_group_rule.cidr_ingress](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |
| [aws_security_group_rule.default_ingress](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/security_group_rule) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_allocated_storage"></a> [allocated\_storage](#input\_allocated\_storage) | The storage capacity for the database | `number` | `20` | no |
| <a name="input_allowed_cidr_blocks"></a> [allowed\_cidr\_blocks](#input\_allowed\_cidr\_blocks) | A list of CIDR blocks that are allowed to access the database | `list(any)` | `[]` | no |
| <a name="input_allowed_security_groups"></a> [allowed\_security\_groups](#input\_allowed\_security\_groups) | A list of Security Group IDs to allow access to the database | `list(any)` | `[]` | no |
| <a name="input_apply_immediately"></a> [apply\_immediately](#input\_apply\_immediately) | Specifies whether any cluster modifications are applied immediately or during the next maintenance window | `bool` | `false` | no |
| <a name="input_backup_retention_period"></a> [backup\_retention\_period](#input\_backup\_retention\_period) | The number of days to retain backups for | `number` | `5` | no |
| <a name="input_backup_window"></a> [backup\_window](#input\_backup\_window) | The time window during which database backups are performed | `string` | `"03:00-06:00"` | no |
| <a name="input_cloudwatch_log_group_retention_in_days"></a> [cloudwatch\_log\_group\_retention\_in\_days](#input\_cloudwatch\_log\_group\_retention\_in\_days) | The number of days to retain CloudWatch logs for the database instance | `number` | `7` | no |
| <a name="input_create_db_subnet_group"></a> [create\_db\_subnet\_group](#input\_create\_db\_subnet\_group) | Whether to create a database subnet group | `bool` | `true` | no |
| <a name="input_create_random_password"></a> [create\_random\_password](#input\_create\_random\_password) | Whether to create a random password for the primary database cluster | `bool` | `true` | no |
| <a name="input_db_name"></a> [db\_name](#input\_db\_name) | The name for an automatically created database on cluster creation | `string` | `""` | no |
| <a name="input_deletion_protection"></a> [deletion\_protection](#input\_deletion\_protection) | Whether accidental deletion protection is enabled | `bool` | `true` | no |
| <a name="input_enable_general_log"></a> [enable\_general\_log](#input\_enable\_general\_log) | Whether to enable general logs in CloudWatch | `bool` | `true` | no |
| <a name="input_enable_slow_query_log"></a> [enable\_slow\_query\_log](#input\_enable\_slow\_query\_log) | Whether to enable slow query logs in CloudWatch | `bool` | `true` | no |
| <a name="input_engine"></a> [engine](#input\_engine) | The name of the database engine to be used for this DB cluster | `string` | `"mysql"` | no |
| <a name="input_engine_version"></a> [engine\_version](#input\_engine\_version) | The database engine version. Updating this argument results in an outage. | `string` | `""` | no |
| <a name="input_environment"></a> [environment](#input\_environment) | Select enviroment type: dev, demo, prod | `string` | `"demo"` | no |
| <a name="input_family"></a> [family](#input\_family) | Version of the MySQL DB family being created | `string` | `""` | no |
| <a name="input_final_snapshot_identifier_prefix"></a> [final\_snapshot\_identifier\_prefix](#input\_final\_snapshot\_identifier\_prefix) | The prefix name for the final snapshot on cluster destroy | `string` | `"final"` | no |
| <a name="input_instance_class"></a> [instance\_class](#input\_instance\_class) | The instance type for the database | `string` | `""` | no |
| <a name="input_kms_key_arn"></a> [kms\_key\_arn](#input\_kms\_key\_arn) | The ARN for the KMS encryption key. If creating an encrypted replica, set this to the destination KMS ARN. If storage\_encrypted is set to true and kms\_key\_id is not specified, the default KMS key created in your account will be used | `string` | `null` | no |
| <a name="input_maintenance_window"></a> [maintenance\_window](#input\_maintenance\_window) | The maintenance window for performing database maintenance | `string` | `"Mon:00:00-Mon:03:00"` | no |
| <a name="input_major_engine_version"></a> [major\_engine\_version](#input\_major\_engine\_version) | The major version of the database engine. Updating this argument results in an outage. | `string` | `""` | no |
| <a name="input_master_username"></a> [master\_username](#input\_master\_username) | The username for the RDS primary cluster | `string` | `""` | no |
| <a name="input_multi_az"></a> [multi\_az](#input\_multi\_az) | Enables multi-AZ for disaster recovery | `bool` | `false` | no |
| <a name="input_port"></a> [port](#input\_port) | The port for the database | `number` | `3306` | no |
| <a name="input_publicly_accessible"></a> [publicly\_accessible](#input\_publicly\_accessible) | Specifies whether the database is publicly accessible over the internet | `bool` | `false` | no |
| <a name="input_random_password_length"></a> [random\_password\_length](#input\_random\_password\_length) | The length of the randomly generated password. (default: 10) | `number` | `20` | no |
| <a name="input_rds_instance_name"></a> [rds\_instance\_name](#input\_rds\_instance\_name) | The name of the RDS instance | `string` | `""` | no |
| <a name="input_replicate_source_db"></a> [replicate\_source\_db](#input\_replicate\_source\_db) | Specifies the identifier of another Amazon RDS Database to replicate as the source database | `string` | `null` | no |
| <a name="input_skip_final_snapshot"></a> [skip\_final\_snapshot](#input\_skip\_final\_snapshot) | Determines whether a final DB snapshot is created before the DB instance is deleted. If true, no DBSnapshot is created. If false, a DB snapshot is created using the value from final\_snapshot\_identifier | `bool` | `true` | no |
| <a name="input_snapshot_identifier"></a> [snapshot\_identifier](#input\_snapshot\_identifier) | Specifies whether to create this database from a snapshot. Use the snapshot ID found in the RDS console, e.g., rds:production-2015-06-26-06-05. | `string` | `null` | no |
| <a name="input_storage_encrypted"></a> [storage\_encrypted](#input\_storage\_encrypted) | Specifies whether database encryption is enabled | `bool` | `true` | no |
| <a name="input_subnet_ids"></a> [subnet\_ids](#input\_subnet\_ids) | A list of subnet IDs used by the database subnet group created | `list(any)` | `[]` | no |
| <a name="input_vpc_id"></a> [vpc\_id](#input\_vpc\_id) | The ID of the VPC where the RDS cluster should be deployed | `string` | `""` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_db_instance_endpoint"></a> [db\_instance\_endpoint](#output\_db\_instance\_endpoint) | The endpoint for connecting to the database instance |
| <a name="output_db_instance_name"></a> [db\_instance\_name](#output\_db\_instance\_name) | The name of the database instance |
| <a name="output_db_instance_password"></a> [db\_instance\_password](#output\_db\_instance\_password) | The password for accessing the database instance (note: Terraform doesn't track changes to this password) |
| <a name="output_db_instance_username"></a> [db\_instance\_username](#output\_db\_instance\_username) | The master username for accessing the database instance |
| <a name="output_db_parameter_group_id"></a> [db\_parameter\_group\_id](#output\_db\_parameter\_group\_id) | The ID of the database parameter group |
| <a name="output_db_secret_arn"></a> [db\_secret\_arn](#output\_db\_secret\_arn) | The ARN (Amazon Resource Name) of the secret storing database credentials |
| <a name="output_db_subnet_group_id"></a> [db\_subnet\_group\_id](#output\_db\_subnet\_group\_id) | The ID of the database subnet group |
| <a name="output_rds_dedicated_security_group"></a> [rds\_dedicated\_security\_group](#output\_rds\_dedicated\_security\_group) | The security group ID associated with the RDS cluster |
<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## Contribute & Issue Report

To report an issue with a project:

  1. Check the repository's [issue tracker](https://github.com/sq-ia/terraform-aws-rds-mysql/issues) on GitHub
  2. Search to check if the issue has already been reported
  3. If you can't find an answer to your question in the documentation or issue tracker, you can ask a question by creating a new issue. Make sure to provide enough context and details.

## License

Apache License, Version 2.0, January 2004 (https://www.apache.org/licenses/LICENSE-2.0)

## Support Us

To support our GitHub project by liking it, you can follow these steps:

  1. Visit the repository: Navigate to the [GitHub repository](https://github.com/sq-ia/terraform-aws-rds-mysql)

  2. Click the "Star" button: On the repository page, you'll see a "Star" button in the upper right corner. Clicking on it will star the repository, indicating your support for the project.

  3. Optionally, you can also leave a comment on the repository or open an issue to give feedback or suggest changes.

Staring a repository on GitHub is a simple way to show your support and appreciation for the project. It also helps to increase the visibility of the project and make it more discoverable to others.

## Who we are

We believe that the key to success in the digital age is the ability to deliver value quickly and reliably. That’s why we offer a comprehensive range of DevOps & Cloud services designed to help your organization optimize its systems & Processes for speed and agility.

  1. We are an AWS Advanced consulting partner which reflects our deep expertise in AWS Cloud and helping 100+ clients over the last 5 years.
  2. Expertise in Kubernetes and overall container solution helps companies expedite their journey by 10X.
  3. Infrastructure Automation is a key component to the success of our Clients and our Expertise helps deliver the same in the shortest time.
  4. DevSecOps as a service to implement security within the overall DevOps process and helping companies deploy securely and at speed.
  5. Platform engineering which supports scalable,Cost efficient infrastructure that supports rapid development, testing, and deployment.
  6. 24*7 SRE service to help you Monitor the state of your infrastructure and eradicate any issue within the SLA.

We provide [support](https://squareops.com/contact-us/) on all of our projects, no matter how small or large they may be.

To find more information about our company, visit [squareops.com](https://squareops.com/), follow us on [Linkedin](https://www.linkedin.com/company/squareops-technologies-pvt-ltd/), or fill out a [job application](https://squareops.com/careers/). If you have any questions or would like assistance with your cloud strategy and implementation, please don't hesitate to [contact us](https://squareops.com/contact-us/).