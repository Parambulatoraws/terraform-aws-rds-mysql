# terraform-aws-rds-mysql

Terraform module to create an AWS RDS (Relational Database Server) MySQL.

## Requirements
* An existing VPC.
* An existing DB Subnet Group.
* An existing Parameter Group for MySQL.
* An existing RDS Enhanced Monitoring role.

### Password for Master Database
* The module will generate a random 16 characteres long password.

### Read Replica Database
If replicate_source_db parameter is defined, it indicates that the instance is meant to be a read replica.

These parameters will be inherited from the master's in the first creation stage:

1. allocated_storage
2. maintenance_window
3. parameter_group_name
3. vpc_security_group_ids

To apply different values for the parameters above, you have to re-apply the configuration after the first creation is finished.

## Usage
```
module "mysql" {
  source = "git::https://github.com/felipefrizzo/terraform-aws-rds-mysql?ref=master"

  product_domain = "frizzo"
  service_name   = "rds-master-frizzo"
  environment    = "production"
  description    = "Mysql server to store Github data"

  instance_class = "db.t2.medium"

  allocated_storage = 100

  # Change to valid security group id
  vpc_security_group_ids = [
    "sg-00000000"
  ]

  # Change to valid db subnet group name
  db_subnet_group_name = "rds-subnet-group"

  # Change to valid parameter group name
  parameter_group_name = "default.mysql5.7"

  maintenance_window      = "Sun:02:00-Sun:03:00"
  backup_window           = "00:00-01:00"
  backup_retention_period = 10

  # Change to valid monitoring role arn
  monitoring_role_arn = "arn:aws:iam::000000000000:role/rds-monitoring-role"
}
```