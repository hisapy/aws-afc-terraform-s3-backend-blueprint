# AWS AFC Terraform S3 Backend blueprint

Use this AWS [Control Tower](https://docs.aws.amazon.com/controltower/) [Account Factory Customization (AFC)](https://docs.aws.amazon.com/controltower/latest/userguide/af-customization-page.html) blueprint to setup a secure and encrypted S3 backend for Terraform when provisioning or updating an AWS account in your **AWS landing zone**.

This blueprint customizes the account with the following resources:

- a private and encrypted S3 bucket with versioning enabled that can be used as a Terraform backend
- an OpenID Connect (OIDC) Provider to connect from GitHub Actions
- an IAM Role assumed when connecting via OIDC from an specific GitHub repository
- an S3 Bucket Policy granting access to the bucket to the above role

## Usage

- Clone this repo or just copy the [terraform-s3-backend.yml](./terraform-s3-backend.yml) template
- Follow the steps in [Customize accounts with Account Factory Customization (AFC)](https://docs.aws.amazon.com/controltower/latest/userguide/af-customization-page.html) using this template providing the required input parameters

Now you can use the ARN of the `GitHubActionsRole` created by this blueprint to authenticate to AWS via OIDC. For example, using the [aws-actions/configure-aws-credentials](https://github.com/aws-actions/configure-aws-credentials), if you store the ARN in `secrets.AWS_ROLE`:

```yaml
- uses: aws-actions/configure-aws-credentials@v5.0.0
  with:
    role-to-assume: ${{ secrets.AWS_ROLE }}
    aws-region: ${{ env.TF_VAR_aws_region }}
```

This ARN looks something like `arn:aws:iam::012312312312:role/GitHubActionsRole`, where **012312312312** is the ID of the provisioned account.

## Why this?

Because I created an AWS Control Tower landing zone, i.e., a well-architected, multi-account AWS environment that is scalable and secure for my clients' workloads but I decided to manage it via the Control Tower UI console because the complexity and cost ($) of automating the process of account provisioning and updating wasn't worth it at this moment. Only a handful of clients, and with no expectation of new ones.

Think of an small software agency, or maybe a solo dev providing software engineering and hosting services to only a handful of clients. _That's me_ 😅

Basically, provision/update accounts via the Control Tower UI using the blueprint to set up accounts to manage their infra resources with Terraform and GitHub Actions.

## NOTES

- The blueprint is stored as an AWS Service Catalog Product which has many versions, a product version is a "provisioning artfifact".

- I created the AFC Hub account under the **Infrastructure** Organizational Unit (OU).

- The accounts for the clients' workloads are provisioned under the **Workloads** OU.

- In the UI console, when creating a new product version, you can upload the template from your computer; however, in the AWS CLI/API, it is ONLY possible to reference the template from an S3 bucket or from a CloudFront stack, and even though `aws servicecatalog create-provisioning-artifact ...` using a GitHub repo URL creates a new product version, when you try to use it, it fails with the error **_The TemplateURL must be a supported URL_**.

- It is not possible to apply a blueprint from the AFC Hub account to the AFC Hub account; I tried this experiment to bootstrap the resources to automate the creation of a new product version from a new release/version on GitHub. I could've manually created the OIDC Provider, S3 bucket, etc., but didn't because I'm not going to change the template anytime soon.

- I think you should have the same product versions as releases names in your GitHub repo, e.g., v0.1.6 product version named after a release of the same name in your GitHub repo.

## Recommended Reading

- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [AWS Control Tower User Guide](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html)
- [Setting up a secure and scalable multi-account AWS environment](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/welcome.html)
- [Organizing Your AWS Environment Using Multiple Accounts](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html)
- [Create a role for OpenID Connect federation (console)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_oidc.html)
- [Provision accounts with AWS Control Tower Accoun Factory for Terraform (AFT)](https://docs.aws.amazon.com/controltower/latest/userguide/taf-account-provisioning.html)
