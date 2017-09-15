Terrascan
==========
A collection of security and best practice tests for static code analysis of (terraform)[https://www.terraform.io] templates using (terraform-validate)[https://github.com/elmundio87/terraform_validate].

Resourced based tests
----------------------
These tests are performed on terraform resources where applicable:
- **Encryption**
    - Server Side Encription (SSE) enabled
    - Use of AWS Key Management Service (KMS) with Customer Managed Keys (CMK)
    - Use of SSL/TLS and proper configuration
- **Security Groups**
    - Provisioning SGs in EC2-classic
    - Ingress open to 0.0.0.0/0
- **Public Exposure**
    - Services with public exposure other than Gateways (NAT, VGW, IGW)
- **Logging & Monitoring**
    - Access logs enabled to resources that support it

Legend:
 - :heavy_minus_sign: - test needs to be implemented
 - :heavy_check_mark: - test implemented
 - blank - N/A

Terraform resource | Encryption | Security Groups | Public Exposure |
------------------ | ---------- | --------------- | --------------- |
aws_alb | | | :heavy_minus_sign: |
aws_alb_listener | :heavy_check_mark: | | |
aws_ami | :heavy_check_mark: | | :heavy_minus_sign: |
aws_ami_copy | :heavy_check_mark: | | |
aws_api_gateway_domain_name | :heavy_check_mark: | | |
aws_cloudfront_distribution | :heavy_check_mark: | | |
aws_cloudtrail | :heavy_check_mark: | | |
aws_codebuild_project | :heavy_check_mark: | | |
aws_codepipeline | :heavy_check_mark: | | |
aws_db_instance | :heavy_check_mark: | | :heavy_minus_sign: |
aws_db_security_group | | :heavy_check_mark: | |
aws_dms_endpoint | :heavy_check_mark: | | |
aws_dms_replication_instance | :heavy_check_mark: | | :heavy_minus_sign: |
aws_ebs_volume | :heavy_check_mark: | | |
aws_efs_file_system | :heavy_check_mark: | | |
aws_elasticache_security_group | | :heavy_check_mark: |
aws_elastictranscoder_pipeline | :heavy_check_mark: | | |
aws_elb | :heavy_check_mark: | | :heavy_minus_sign: |
aws_emr_cluster | | | :heavy_minus_sign: |
aws_instance | :heavy_check_mark: | | :heavy_minus_sign: |
aws_kinesis_firehose_delivery_stream | :heavy_check_mark: | | |
aws_lambda_function | :heavy_check_mark: | | |
aws_launch_configuration | | | :heavy_minus_sign: |
aws_lb_ssl_negotiation_policy | :heavy_minus_sign: | | |
aws_load_balancer_backend_server_policy | :heavy_minus_sign: | | |
aws_load_balancer_listener_policy | :heavy_minus_sign: | | |
aws_load_balancer_policy | :heavy_minus_sign: | | |
aws_opsworks_application | :heavy_check_mark: | | :heavy_minus_sign: |
aws_opsworks_custom_layer | | | :heavy_minus_sign: |
aws_opsworks_ganglia_layer | | | :heavy_minus_sign: |
aws_opsworks_haproxy_layer | | | :heavy_minus_sign: |
aws_opsworks_instance | | | :heavy_minus_sign: |
aws_opsworks_java_app_layer | | | :heavy_minus_sign: |
aws_opsworks_memcached_layer | | | :heavy_minus_sign: |
aws_opsworks_mysql_layer | | | :heavy_minus_sign: |
aws_opsworks_nodejs_app_layer | | | :heavy_minus_sign: |
aws_opsworks_php_app_layer | | | :heavy_minus_sign: |
aws_opsworks_rails_app_layer | | | :heavy_minus_sign: |
aws_opsworks_static_web_layer | | | :heavy_minus_sign: |
aws_rds_cluster | :heavy_check_mark: | | |
aws_rds_cluster_instance | | | :heavy_minus_sign: |
aws_redshift_cluster | :heavy_check_mark: | | :heavy_minus_sign: |
aws_redshift_parameter_group | :heavy_minus_sign: | | |
aws_redshift_security_group | | :heavy_check_mark: | |
aws_s3_bucket | | | :heavy_minus_sign: |
aws_s3_bucket_object | :heavy_check_mark: | | |
aws_security_group | | :heavy_check_mark: |
aws_security_group_rule | | :heavy_check_mark: | |
aws_ses_receipt_rule | :heavy_minus_sign: | | |
aws_spot_instance_request | | | :heavy_minus_sign: |
aws_sqs_queue | :heavy_check_mark: | | :heavy_minus_sign: |
aws_ssm_parameter | :heavy_check_mark: | | |

Identity and access management
------------------------------
Checks for overly permissive permissions and bad practices.
Verifies that:
- For each of these types of policies that there are no NotActions:
    - IAM policy
    - IAM role trust relationship
    - S3 bucket policy
    - SNS topic policy
    - SQS queue policy
    - KMS policy
- For each of these types of policies that there are no NotPrincipals:
    - IAM role trust relationship
    - S3 bucket policy
    - SNS topic policy
    - SQS queue policy
    - KMS policy
- For each of these types of policies that there are no wildcard actions:
    - IAM policy
    - IAM role trust relationship
    - S3 bucket policy
    - SQS queue policy
    - KMS policy
- For each of these types of policies that there are no wildcard principals:
    - Lambda permission
    - S3 bucket policy
    - SNS topic policy
    - SQS queue policy
    - KMS policy
- No policies attached to IAM users
- No inline policies on:
    - IAM users
    - IAM roles
- S3 bucket no public-read ACL
- S3 bucket no public-read-write ACL
- S3 bucket no authenticated-read ACL
- The AWS administrator managed policy shouldn't be attached to any resources
- AWS Managed policies can't be scanned
- No creation of IAM API keys

Governance best practices
-------------------------
Checks against general governance best practices.
Verifies that:
- A specified number of tags are applied to all resources when supported.
- Autoscaling lifecycle actions are enabled to reduce uneccessary cost on unused resources
- There are no EC2 instance types provisioned for which AWS doesn't allow penetration testing: m1.small, t1.micro, or t2.nano
- There are no RDS instance types provisiones for which AWS doesn't allow penetration testing: small, micro
- Only approved AMIs are provisioned
- No S3 bucket names larger than 63 characters
- There are no hardcoded credentials in terraform templates
