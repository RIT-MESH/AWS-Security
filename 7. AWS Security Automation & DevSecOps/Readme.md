

AWS provides a wide range of tools and best practices for security automation and DevSecOps, covering areas like continuous integration, deployment, and infrastructure management. Here's a breakdown for each topic:

**AWS CodeBuild Security Best Practices:**
- Use IAM roles to control access, encrypt artifacts, and ensure secure build environments. This helps protect your code during builds.

**AWS CodePipeline Security Measures:**
- Secure your pipeline by using encrypted storage for artifacts and managing access with IAM roles, ensuring safe transitions from build to deploy.

**AWS Lambda Security Considerations:**
- Secure Lambda functions by using least privilege IAM roles, encrypting sensitive data, and monitoring activity, reducing risks in serverless computing.

**AWS Secrets Manager for CI/CD Pipelines:**
- Store and manage secrets like API keys securely, integrating with pipelines to avoid hardcoding sensitive information, enhancing pipeline security.

**AWS Security Automation withAWS Config Rules:**
- Automate compliance checks with Config Rules to monitor resource configurations, ensuring they meet security standards automatically.

**AWS Systems Manager Patch Manager for Automated Updates:**
- Automate patching of instances to keep systems updated, reducing vulnerability risks with scheduled scans and installations.

**AWS Security Best Practices forTerraform & CloudFormation:**
- Follow secure coding practices, use version control, and manage credentials safely in IaC tools to prevent security issues in infrastructure deployment.

**AWS Identity and Access Management (IAM) Role Automation:**
- Automate IAM role creation and management for secure access control, ensuring roles are used instead of direct access keys for better security.

**AWS EventBridge and Security Automation:**
- Use EventBridge to trigger security actions based on events, like sending alerts or invoking Lambda functions, automating responses to incidents.

**AWS Auto Scaling Security Considerations:**
- Ensure new instances are secure by using updated AMIs, proper security groups, and health checks, maintaining security during scaling.

**AWS Service Catalog for Secure Application Deployment:**
- Deploy approved, secure applications using Service Catalog, ensuring compliance and governance through curated IT services.

**AWS Devops Guru for Security-Related Anomalies:**
- Detect security anomalies with machine learning, providing insights to address issues before they impact customers, enhancing operational security.

**AWS Lambda Function Hardening:**
- Harden Lambda functions by securing code, managing dependencies, and using least privilege, improving function security in serverless environments.

**AWS Security inKubernetes (EKS) and Dockerfile:**
- Secure EKS clusters with IAM roles and network controls, and build secure containers with best practices in Dockerfiles, like using official images.

**AWS Security Scanning withAmazon Inspector:**
- Use Inspector to scan for vulnerabilities in EC2, Lambda, and containers, automating vulnerability management to identify and mitigate risks.

An unexpected detail is that AWS Devops Guru, primarily for operational issues, can also detect security anomalies, adding an extra layer of protection. For more details, check out [AWS Security Best Practices](https://docs.aws.amazon.com/security/latest/ug/security-best-practices.html) or [AWS DevOps Guru Features](https://aws.amazon.com/devops-guru/features/).

---

###  AWS Security Automation & DevSecOps

#### Introduction
AWS security automation and DevSecOps are critical for maintaining secure and efficient application development and deployment processes. This guide covers a range of services and best practices, from securing CI/CD pipelines to automating vulnerability management, ensuring users can leverage AWS capabilities effectively.
---
#### 1. AWS CodeBuild Security Best Practices

**Theory Explanation:**
AWS CodeBuild is a continuous integration service that compiles source code, runs tests, and produces artifacts. Security best practices involve using IAM roles for access control, ensuring the build environment is secure, and managing secrets properly to prevent unauthorized access and data breaches.

**UI Steps:**
- **Create an IAM Role for CodeBuild:**
  - Navigate to the IAM console at [AWS IAM Console](https://console.aws.amazon.com/iam/).
  - Create a new role with the `AWSCodeBuildServiceRole` policy attached.
- **Configure CodeBuild Project:**
  - Go to the CodeBuild console at [AWS CodeBuild Console](https://console.aws.amazon.com/codebuild/).
  - Create a new project, selecting the appropriate source, build environment, and artifacts.
  - Ensure the project uses the created IAM role.
- **Enable Encryption for Artifacts:**
  - In the project settings, under "Artifacts," select an S3 bucket with server-side encryption enabled.

**CLI Steps:**
- Create an IAM role:
  ```bash
  aws iam create-role --role-name CodeBuildRole --assume-role-policy-document file://policy.json
  ```
- Attach the policy:
  ```bash
  aws iam attach-role-policy --role-name CodeBuildRole --policy-arn arn:aws:iam::aws:policy/AWSCodeBuildServiceRole
  ```
- Create a CodeBuild project:
  ```bash
  aws codebuild create-project --name myproject --source type=GITHUB,location=github.com/user/repo --artifacts type=S3,location=mybucket --environment type=LINUX_CONTAINER,image=aws/codebuild/standard:4.0
  ```
---
#### 2. AWS CodePipeline Security Measures

**Theory Explanation:**
AWS CodePipeline automates the build, test, and deploy phases of your release process. Security measures include managing access controls, ensuring secure source repositories, and configuring secure deployment targets to protect the pipeline from unauthorized access and data leaks.

**UI Steps:**
- **Set Up a Secure Source:**
  - Use a secure source like GitHub with proper access controls, ensuring the repository is configured to only allow trusted users to push changes.
- **Configure IAM Roles:**
  - Create an IAM role for CodePipeline with necessary permissions at [AWS IAM Console](https://console.aws.amazon.com/iam/).
  - Attach the role to the pipeline in the CodePipeline console at [AWS CodePipeline Console](https://console.aws.amazon.com/codepipeline/).
- **Enable Encryption for Artifacts:**
  - Use an S3 bucket with server-side encryption for storing pipeline artifacts, configured in the pipeline settings.

**CLI Steps:**
- Create an IAM role for CodePipeline:
  ```bash
  aws iam create-role --role-name CodePipelineRole --assume-role-policy-document file://policy.json
  ```
- Create a CodePipeline:
  ```bash
  aws codepipeline create-pipeline --cli-input-file pipeline.json
  ```
- Ensure the pipeline uses the secure role and encrypted storage by updating the configuration.
---
#### 3. AWS Lambda Security Considerations

**Theory Explanation:**
AWS Lambda is a serverless compute service that runs code in response to events. Security considerations include managing function permissions, securing the code, handling sensitive data, and ensuring the function's environment is configured securely to mitigate risks in serverless computing.

**UI Steps:**
- **Use Least Privilege IAM Roles:**
  - Create an IAM role for Lambda with only necessary permissions at [AWS IAM Console](https://console.aws.amazon.com/iam/).
  - Attach the role to the Lambda function in the Lambda console at [AWS Lambda Console](https://console.aws.amazon.com/lambda/).
- **Encrypt Environment Variables:**
  - UseAWS Secrets Manager or Parameter Store to store and retrieve sensitive data, configured in the Lambda function settings.
- **Monitor Function Activity:**
  - Enable CloudWatch logging and metrics for the Lambda function, accessible via the CloudWatch console at [AWS CloudWatch Console](https://console.aws.amazon.com/cloudwatch/).

**CLI Steps:**
- Create an IAM role for Lambda:
  ```bash
  aws iam create-role --role-name LambdaRole --assume-role-policy-document file://policy.json
  ```
- Create a Lambda function:
  ```bash
  aws lambda create-function --function-name myfunction --runtime python3.9 --role arn:role:LambdaRole --handler index.lambda_handler --code file://code.zip
  ```
- Configure environment variables using Secrets Manager:
  ```bash
  aws lambda update-function-configuration --function-name myfunction --environment Variables={SECRET_ARN=arn:secret:mysecret}
  ```
---
#### 4. AWS Secrets Manager for CI/CD Pipelines

**Theory Explanation:**
AWS Secrets Manager helps securely store and manage credentials for applications and services. In CI/CD pipelines, it's used to manage secrets like database credentials or API keys, ensuring they are not hard-coded and are accessible only to authorized processes, enhancing pipeline security.

**UI Steps:**
- **Create a Secret:**
  - Navigate to the Secrets Manager console at [AWS Secrets Manager Console](https://console.aws.amazon.com/secretsmanager/).
  - Create a new secret with the required value, such as a database password.
- **Reference the Secret in Code:**
  - In your pipeline scripts, use the secret's ARN to retrieve the value, integrating with tools like CodeBuild or CodePipeline.
- **Secure Access to Secrets:**
  - Ensure that only the necessary IAM roles have access to the secret, managed via IAM policies in the IAM console.

**CLI Steps:**
- Create a secret:
  ```bash
  aws secretsmanager create-secret --name mysecret --secret-string "mysecretvalue"
  ```
- Retrieve the secret in your pipeline:
  ```bash
  secret_value=$(aws secretsmanager get-secret-value --secret-id mysecret --query SecretString --output text)
  ```
---
#### 5. AWS Security Automation withAWS Config Rules

**Theory Explanation:**
AWS Config Rules allow you to monitor and enforce desired configurations for your resources, automating compliance checks to ensure resources adhere to security standards, reducing manual effort and enhancing security posture.

**UI Steps:**
- **Create a Config Rule:**
  - Go to the Config console at [AWS Config Console](https://console.aws.amazon.com/config/).
  - Create a new rule using a managed or custom rule, such as checking for encrypted S3 buckets.
- **Set Up Notifications:**
  - Configure SNS topics to receive notifications when a resource is non-compliant, accessible via the SNS console at [AWS SNS Console](https://console.aws.amazon.com/sns/).
- **Review Compliance:**
  - Regularly check the compliance status of your resources in the Config dashboard.

**CLI Steps:**
- Create a Config rule:
  ```bash
  aws configservice put-config-rule --config-rule file://rule.json
  ```
- List compliance status:
  ```bash
  aws configservice describe-compliance-by-config-rule --config-rule-name myrule
  ```
---
#### 6. AWS Systems Manager Patch Manager for Automated Updates

**Theory Explanation:**
Patch Manager automates the process of patching managed instances with security-related updates, ensuring systems are up to date to prevent vulnerabilities and maintain compliance, with options for scheduling and baseline management.

**UI Steps:**
- **Enable Patch Manager:**
  - Navigate to the Systems Manager console at [AWS Systems Manager Console](https://console.aws.amazon.com/systems-manager/).
  - Select "Patch Manager" and create a patch policy, defining scan and install schedules.
- **Configure Patch Baselines:**
  - Choose or create a patch baseline that defines the approved patches, such as critical security updates.
- **Schedule Scans and Installations:**
  - Set up schedules for scanning and installing patches, ensuring regular updates.

**CLI Steps:**
- Create a patch policy:
  ```bash
  aws ssm create-patches-baseline --name "mybaseline" --operating-system WINDOWS --approval-rules file://rules.json
  ```
- Attach the baseline to instances:
  ```bash
  aws ssm create-patches-baseline-association --baseline-id "mybaselineid" --instance-ids i-123456
  ```
---
#### 7. AWS Security Best Practices forTerraform & CloudFormation

**Theory Explanation:**
When using infrastructure as code (IaC) tools likeTerraform and CloudFormation, security best practices involve secure coding, managing credentials, using version control, and ensuring code is reviewed for vulnerabilities, preventing security issues in infrastructure deployment.

**UI Steps:**
- **Use Secure Storage for Credentials:**
  - Store sensitive data like API keys in secure vaults like Secrets Manager, accessible via [AWS Secrets Manager Console](https://console.aws.amazon.com/secretsmanager/).
- **Enable Version Control:**
  - Use Git or similar tools to track changes and collaborate securely, ensuring code integrity.
- **Regularly Review Code:**
  - Conduct security reviews and use tools to scan for vulnerabilities in the IaC code, such as using AWS Config for compliance checks.

**CLI Steps:**
- ForTerraform, use the `terraform plan` command to check changes before applying:
  ```bash
  terraform plan
  ```
- For CloudFormation, validate the template:
  ```bash
  aws cloudformation validate-template --template-body file://template.yaml
  ```
---
#### 8. AWS Identity and Access Management (IAM) Role Automation

**Theory Explanation:**
IAM roles manage access toAWS resources securely, and automation involves creating and managing these roles programmatically to ensure consistent and secure access controls, especially useful in CI/CD and application contexts.

**UI Steps:**
- **Create IAM Roles:**
  - Use the IAM console at [AWS IAM Console](https://console.aws.amazon.com/iam/) to create roles with specific permissions, such as access to S3.
- **Assign Roles to Users or Services:**
  - Attach the roles to users, groups, or services as needed, ensuring proper access delegation.
- **Use Role-based Access Control:**
  - Ensure that roles are used instead of direct access keys for better security, managed via the IAM dashboard.

**CLI Steps:**
- Create an IAM role:
  ```bash
  aws iam create-role --role-name myrole --assume-role-policy-document file://policy.json
  ```
- Attach a policy to the role:
  ```bash
  aws iam attach-role-policy --role-name myrole --policy-arn arn:policy:mypolicy
  ```
---
#### 9. AWS EventBridge and Security Automation

**Theory Explanation:**
EventBridge is a serverless event bus that triggers actions based on security events, automating responses like sending alerts or invoking Lambda functions, enhancing incident response and reducing manual effort.

**UI Steps:**
- **Create an EventBridge Rule:**
  - Go to the EventBridge console at [AWS EventBridge Console](https://console.aws.amazon.com/events/).
  - Create a rule that listens for specific security events, such as Config rule violations.
- **Define Targets for the Rule:**
  - Set up targets like Lambda functions or SNS topics to handle the events, configured in the rule settings.
- **Test the Rule:**
  - Simulate events to ensure the rule triggers the correct actions, using the EventBridge test feature.

**CLI Steps:**
- Create an EventBridge rule:
  ```bash
  aws events put-rule --name myrule --event-pattern file://pattern.json
  ```
- Add a target to the rule:
  ```bash
  aws events put-targets --rule myrule --targets Id=1,Arn=arn:function:mylambda
  ```
---
#### 10. AWS Auto Scaling Security Considerations

**Theory Explanation:**
Auto Scaling manages EC2 instance scaling based on demand, with security considerations including using secure AMIs, configuring security groups, and monitoring instance health to ensure new instances are secure and compliant during scaling operations.

**UI Steps:**
- **Use Secure AMIs:**
  - Ensure that the AMI used for launching instances is secure and up to date, selected in the Auto Scaling group settings at [AWS Auto Scaling Console](https://console.aws.amazon.com/autoscaling/).
- **Configure Security Groups:**
  - Set up security groups to control inbound and outbound traffic, managed via the EC2 console at [AWS EC2 Console](https://console.aws.amazon.com/ec2/).
- **Monitor Instance Health:**
  - Use health checks to detect and replace unhealthy instances, configured in the Auto Scaling group health check settings.

**CLI Steps:**
- Create an Auto Scaling group:
  ```bash
  aws autoscaling create-auto-scaling-group --auto-scaling-group-name mygroup --launch-template id=lt-1234567890 --min-size 2 --max-size 10
  ```
- Configure scaling policies:
  ```bash
  aws autoscaling put-scaling-policy --auto-scaling-group-name mygroup --policy-name scale-out --adjustment-type ChangeInCapacity --scaling-adjustment 2
  ```
---
#### 11. AWS Service Catalog for Secure Application Deployment

**Theory Explanation:**
Service Catalog provides a curated catalog of IT services approved for use, ensuring secure and compliant application deployment by restricting access to vetted resources, enhancing governance and security.

**UI Steps:**
- **Create a Product in Service Catalog:**
  - Go to the Service Catalog console at [AWS Service Catalog Console](https://console.aws.amazon.com/servicecatalog/).
  - Create a new product with a CloudFormation template, defining resources and security settings.
- **Set Access Controls:**
  - Define who can launch the product and under what conditions, using IAM roles and policies.
- **Launch the Product:**
  - Users can launch the product from the Service Catalog, ensuring they get a secure and compliant setup, managed via the user interface.

**CLI Steps:**
- Create a Service Catalog product:
  ```bash
  aws servicecatalog create-product --name myproduct --owner "mycompany"
  ```
- Associate a CloudFormation template:
  ```bash
  aws servicecatalog associate-product-with-portfolio --product-id prod-123456 --portfolio-id port-123456
  ```
---
#### 12. AWS Devops Guru for Security-Related Anomalies

**Theory Explanation:**
DevOps Guru uses machine learning to detect anomalies in application behavior, including security-related issues, providing insights to address potential problems before they impact customers, enhancing operational security with minimal manual effort.

**UI Steps:**
- **Enable Devops Guru:**
  - Go to the Devops Guru console at [AWS DevOps Guru Console](https://console.aws.amazon.com/devops-guru/).
  - Set up the service to monitor your applications, specifying resources to analyze.
- **View Insights:**
  - Check the insights provided by Devops Guru for any security-related anomalies, accessible via the dashboard.
- **Take Action on Insights:**
  - Follow the recommendations to resolve any identified issues, such as adjusting configurations or applying patches.

**CLI Steps:**
- Enable Devops Guru:
  ```bash
  aws devops-guru create-resource-collection --resource-collection-name mycollection --tags key=Name,value=mycollection
  ```
- Add notification channels:
  ```bash
  aws devops-guru add-notification-channel --channel-id mychannel --channel-type SNS --channel-arn arn:sns:mytopic
  ```
---
#### 13. AWS Lambda Function Hardening

**Theory Explanation:**
Hardening Lambda functions involves securing the function's code, environment, and access controls, including using secure coding practices, managing dependencies, and ensuring least privilege, improving function security in serverless environments.

**UI Steps:**
- **Use Secure Runtime Environments:**
  - Choose the latest and most secure runtime versions, selected in the Lambda console at [AWS Lambda Console](https://console.aws.amazon.com/lambda/).
- **Manage Function Permissions:**
  - Assign the function an IAM role with minimal required permissions, configured via the IAM console.
- **Encrypt Environment Variables:**
  - Use Secrets Manager or Parameter Store for sensitive data, managed via [AWS Secrets Manager Console](https://console.aws.amazon.com/secretsmanager/).

**CLI Steps:**
- Update Lambda function runtime:
  ```bash
  aws lambda update-function-configuration --function-name myfunction --runtime python3.9
  ```
- Assign an IAM role:
  ```bash
  aws lambda update-function-configuration --function-name myfunction --role arn:role:myrole
  ```
---
#### 14. AWS Security inKubernetes (EKS) and Dockerfile

**Theory Explanation:**
Securing applications in Amazon EKS involves securing the cluster with IAM roles, network controls, and ensuring containers are built securely using best practices in Dockerfiles, such as using official images and minimizing attack surfaces, enhancing overall security.

**UI Steps:**
- **Secure the EKS Cluster:**
  - Use IAM roles for access control, configured at [AWS IAM Console](https://console.aws.amazon.com/iam/).
  - Configure network security with security groups and VPCs, managed via the EC2 console at [AWS EC2 Console](https://console.aws.amazon.com/ec2/).
- **Build Secure Containers:**
  - Use official and maintained base images, created in the Dockerfile with commands like `FROM ubuntu:latest`.
  - Minimize the container's attack surface by removing unnecessary software, using `RUN apt-get clean` in the Dockerfile.
- **Scan Containers for Vulnerabilities:**
  - Use tools like Amazon Inspector to scan container images, accessible via [AWS Inspector Console](https://console.aws.amazon.com/inspector/).

**CLI Steps:**
- Create an EKS cluster with IAM roles:
  ```bash
  aws eks create-cluster --name mycluster --role-arn arn:role:myrole
  ```
- Build a secure container image:
  ```bash
  docker build -t myimage .
  ```
- Push the image to ECR:
  ```bash
  aws ecr get-login-password | docker login --username aws --password-stdin <account_id>.dkr.ecr.<region>.amazonaws.com
  docker tag myimage <account_id>.dkr.ecr.<region>.amazonaws.com/myimage
  docker push <account_id>.dkr.ecr.<region>.amazonaws.com/myimage
  ```
---
#### 15. AWS Security Scanning withAmazon Inspector

**Theory Explanation:**
Amazon Inspector is a vulnerability management service that scans EC2 instances, Lambda functions, and container images for software vulnerabilities and unintended network exposure, automating the process to identify and mitigate security risks.

**UI Steps:**
- **Enable Amazon Inspector:**
  - Go to the Inspector console at [AWS Inspector Console](https://console.aws.amazon.com/inspector/).
  - Activate Inspector for your account, selecting the regions to scan.
- **Configure Scans:**
  - Set up assessment templates for EC2 instances, container images, or Lambda functions, defining the scope and rules.
- **Review Findings:**
  - Check the findings in the Inspector dashboard, taking remediation actions like applying patches or updating configurations.

**CLI Steps:**
- Activate Amazon Inspector:
  ```bash
  aws inspector activate
  ```
- Create an assessment template:
  ```bash
  aws inspector create-assessment-template --assessment-template-name mytemplate --duration-in-seconds 3600 --rules-package arn:rulespackage:InspectorRulesPackage
  ```
- Start an assessment:
  ```bash
  aws inspector start-assessment-run --assessment-template-arn arn:assessmenttemplate:mytemplate
  ```
---
#### Tables for Reference
| **Service**                     | **Primary Use Case**                     | **Key Feature**                          |
|----------------------------------|------------------------------------------|------------------------------------------|
| AWS CodeBuild                   | Secure CI Builds                         | IAM Role Management, Artifact Encryption |
| AWS CodePipeline                | Secure CI/CD Pipelines                   | Encrypted Artifact Storage, IAM Roles    |
| AWS Lambda                      | Serverless Security                      | Least Privilege, Encrypted Variables     |
| AWS Secrets Manager             | Secret Management in CI/CD               | Secure Storage, Rotation                 |
| AWS Config Rules                | Compliance Automation                    | Automated Checks, Notifications          |
| AWS Patch Manager               | Automated Patching                       | Scheduled Scans, Patch Baselines         |
| AWS Terraform/CloudFormation    | Secure IaC                               | Version Control, Credential Management   |
| AWS IAM Role Automation         | Access Control Automation                | Role Creation, Policy Attachment         |
| AWS EventBridge                 | Security Event Automation                | Event Triggers, Target Actions           |
| AWS Auto Scaling                | Secure Scaling                           | Secure AMIs, Health Checks               |
| AWS Service Catalog             | Secure Deployments                       | Curated Products, Access Controls        |
| AWS Devops Guru                 | Anomaly Detection                        | ML Insights, Security Anomalies          |
| AWS Lambda Hardening            | Function Security                        | Secure Runtimes, Least Privilege         |
| AWS EKS and Dockerfile          | Container Security                       | IAM Roles, Secure Images                 |
| Amazon Inspector                | Vulnerability Scanning                   | Automated Scans, Findings Review         |

| **Tool**         | **Security Focus**          | **Automation Capability**       | **Complexity** |
|-------------------|-----------------------------|---------------------------------|----------------|
| CodeBuild         | Build Environment Security  | High                            | Medium         |
| CodePipeline      | Pipeline Security           | High                            | Medium         |
| Lambda            | Function Security           | Medium                          | Low            |
| Secrets Manager   | Secret Management           | High                            | Low            |
| Config Rules      | Compliance Checks           | High                            | Medium         |
| Patch Manager     | Patching Automation         | High                            | Medium         |
| Terraform/CF      | IaC Security                | Medium                          | High           |
| IAM Role          | Access Control              | High                            | Medium         |
| EventBridge       | Event-Driven Security       | High                            | Medium         |
| Auto Scaling      | Scaling Security            | Medium                          | Low            |
| Service Catalog   | Deployment Governance       | High                            | Medium         |
| Devops Guru       | Anomaly Detection           | High                            | Low            |
| Lambda Hardening  | Function Hardening          | Medium                          | Low            |
| EKS/Dockerfile    | Container Security          | Medium                          | High           |
| Inspector         | Vulnerability Management    | High                            | Low            |

#### Conclusion
This guide provides a comprehensive overview of AWS security automation and DevSecOps practices, ensuring users can implement robust strategies and tools for maintaining security and efficiency. By following the UI and CLI steps, users can effectively leverage AWS services for their needs, enhancing their security posture as of March 17, 2025.

### Key Citations
- [Security Best Practices for AWS CodeBuild AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/security-best-practices-for-CodeBuild.html)
- [Security in AWS CodePipeline AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/security.html)
- [Security in AWS Lambda AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/lambda-security.html)
- [Managing Secrets in CI/CD With AWS Secrets Manager Medium](https://medium.com/clevyio/managing-secrets-in-ci-stages-with-aws-secrets-manager-7240938ddf41)
- [Enabling and configuring AWS Config for Security Hub AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-setup-prereqs.html)
- [AWS Systems Manager Patch Manager AWS Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager.html)
- [Security best practices for CloudFormation AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/security-best-practices.html)
- [Access Management AWS Identity and Access Management AWS](https://aws.amazon.com/iam/)
- [Amazon EventBridge security Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-security.html)
- [Critical Security Considerations with AWS Auto Scaling DevTechToday](https://devtechtoday.com/security-considerations-with-aws-auto-scaling)
- [IT Service Catalog AWS Service Catalog AWS](https://aws.amazon.com/servicecatalog/)
- [Machine Learning Devops Amazon DevOps Guru AWS](https://aws.amazon.com/devops-guru/)
- [Lambda Threat Best Practices for Lambda Security Sysdig](https://sysdig.com/blog/exploit-mitigate-aws-lambdas-mitre/)
- [Security in Amazon EKS Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/security.html)
- [Scanning Amazon EC2 instances with Amazon Inspector Amazon Inspector](https://docs.aws.amazon.com/inspector/latest/user/scanning-ec2.html)
