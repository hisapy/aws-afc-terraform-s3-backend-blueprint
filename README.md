# AWS Control Tower Landing Zone

Initial terraform config to set up an AWS Control Tower landing zone on AWS.

The base Control Tower was:

1. Created on the AWS console with the default options, on an account without prior organizations
2. Imported to Terraform `terraform import`

Overall, the config is mostly based on recommendations from the following docs:

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [AWS Control Tower User Guide](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html)
- [Organizing Your AWS Environment Using Multiple Accounts](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html)
