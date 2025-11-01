# AWS AFC Terraform S3 Backend blueprint

Use this AWS [Control Tower](https://docs.aws.amazon.com/controltower/) [Account Factory Customization (AFC)](https://docs.aws.amazon.com/controltower/latest/userguide/af-customization-page.html) blueprint to setup a secure and encrypted S3 backend for Terraform when provisioning or updating an AWS account in your AWS landing zone.

This blueprint customizes the account with the following resources:

- a private and encrypted S3 bucket with versioning enabled that can be used as a Terraform backend
- an OpenID Connect (OIDC) Provider to connect from GitHub Actions
- an IAM Role assumed when connecting via OIDC from an specific GitHub repository
- a S3 Bucket Policy granting access to the bucket to the above role

## Recommended Reading

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [AWS Control Tower User Guide](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html)
- [Setting up a secure and scalable multi-account AWS environment](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/welcome.html)
- [Organizing Your AWS Environment Using Multiple Accounts](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html)
- [Create a role for OpenID Connect federation (console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_oidc.html)
