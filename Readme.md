
## Setup

### Provisioning Distribution

AWS Roles seem like a simple and secure way to control access to Git resources without having to distribute credentials and actually connect to github.

1. Create S3 Bucket for Provisioning Repo
   I created `bloom-dev-provisioning-deployment` for the PofC.

1. Create an IAM Role.  I used the wizard. 
   1. Under "AWS Service Roles" I created an "Amazon EC2" type.
   2. Select "Custom Policy" and add this policy ( which I called `ProvisioningTarballAccess` ):
      <pre>
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Action": [
              "s3:GetObject",
              "s3:GetObjectVersion",
              "s3:ListBucket",
              "s3:ListBucketVersions",
              "s3:PutObject"
            ],
            "Resource": [
              "arn:aws:s3:::bloom-dev-provisioning-deployment",
              "arn:aws:s3:::bloom-dev-provisioning-deployment/",
              "arn:aws:s3:::bloom-dev-provisioning-deployment/*"
            ],
            "Effect": "Allow"
          }
        ]
      }
      </pre>
   3. Continue through the wizard to create the role.
   4. Grab the ARN for use below.

1. Add an Instance Profile to the CloudFormation template.  
   Each instance that should have the role should hold its role _name_ under "IamInstanceProfile".
   For me, that was 
   <pre>
   TestStack-ProvisioningDeployment
   </pre>
   I am not sure this is correct yet.  It _appears_ that the ARN or "Resource name" can be set here instead of 
   creating a role and profile within cloudFormation.

1. Once the instance is booted, the IAM credentails are accessible under
   http://169.254.169.254/latest/meta-data/iam/security-credentials/_role_
   For me this would be
   <pre>
   curl http://169.254.169.254/latest/meta-data/iam/security-credentials/TestStack-ProvisioningDeployment
   </pre>
   IAM credentails there are auto-rotated.  
