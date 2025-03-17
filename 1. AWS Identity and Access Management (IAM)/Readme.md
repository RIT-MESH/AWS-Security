**Comprehensive AWS Security Topics**

### **1. AWS Identity and Access Management (IAM)**

#### **IAM Users, Groups, Roles, and Policies**
**Theory:** IAM enables secure access control through Users, Groups, Roles, and Policies. Users represent individual identities, Groups allow managing multiple users, Roles provide temporary access, and Policies define permissions.

**UI Steps:**
1. Navigate to AWS IAM Console.
2. Click on "Users" → "Add User" to create a user.
3. Assign the user to a group or create a new group with policies.
4. Navigate to "Roles" → "Create Role" and define trust relationships.
5. Attach managed or custom policies to the role.
6. Save and test the user’s access.

**CLI Steps:**
```sh
aws iam create-user --user-name MyUser
aws iam create-group --group-name MyGroup
aws iam add-user-to-group --user-name MyUser --group-name MyGroup
aws iam create-role --role-name MyRole --assume-role-policy-document file://trust-policy.json
aws iam attach-role-policy --role-name MyRole --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

---

#### **IAM Best Practices**
**Theory:** Best practices include enabling MFA, using least privilege access, rotating credentials, using IAM roles instead of root access, and monitoring IAM activity.

**UI Steps:**
1. Enable MFA for root and IAM users.
2. Use IAM roles instead of static credentials.
3. Regularly review IAM permissions via IAM Access Analyzer.
4. Use AWS Organizations to centralize security management.

**CLI Steps:**
```sh
aws iam update-account-password-policy --require-uppercase-characters --minimum-password-length 14 --require-symbols
aws iam enable-mfa-device --user-name MyUser --serial-number arn:aws:iam::123456789012:mfa/MyUser --authentication-code-1 123456 --authentication-code-2 456789
```

---

#### **IAM Policy Evaluation Logic**
**Theory:** IAM policies are evaluated based on explicit denies, explicit allows, and implicit denies.

**UI Steps:**
1. Open IAM Console → "Policies".
2. Select or create a policy.
3. Review JSON structure and permissions.

**CLI Steps:**
```sh
aws iam get-policy --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
```

---

#### **IAM Conditions and Permission Boundaries**
**Theory:** IAM conditions define the circumstances under which permissions are granted. Permission boundaries limit the maximum permissions a user or role can have.

**UI Steps:**
1. Open IAM Console → "Policies".
2. Edit policy and define condition keys.
3. Attach the permission boundary to the role.

**CLI Steps:**
```sh
aws iam put-user-permissions-boundary --user-name MyUser --permissions-boundary arn:aws:iam::aws:policy/PowerUserAccess
```

---

#### **AWS Organizations and Service Control Policies (SCPs)**
**Theory:** AWS Organizations allows centralized management of accounts, and SCPs enforce security policies across them.

**UI Steps:**
1. Navigate to AWS Organizations Console.
2. Create an Organization and add accounts.
3. Create and attach SCPs to Organizational Units (OUs).

**CLI Steps:**
```sh
aws organizations create-policy --content file://scp.json --name "DenyAllExceptBilling" --type SERVICE_CONTROL_POLICY
aws organizations attach-policy --policy-id p-examplepolicyid --target-id o-exampleorgid
```

---

#### **IAM Access Analyzer**
**Theory:** IAM Access Analyzer helps detect overly permissive policies and improves security posture.

**UI Steps:**
1. Open IAM Console → "Access Analyzer".
2. Click "Create Analyzer".
3. Review findings and adjust policies accordingly.

**CLI Steps:**
```sh
aws accessanalyzer create-analyzer --analyzer-name MyAnalyzer --type ACCOUNT
aws accessanalyzer list-findings --analyzer-name MyAnalyzer
```

---

#### **AWS Single Sign-On (SSO)**
**Theory:** AWS SSO allows centralized access management across AWS accounts and third-party apps.

**UI Steps:**
1. Enable AWS SSO from the Management Console.
2. Configure user access and permissions.
3. Integrate with external identity providers (Okta, AD, etc.).

**CLI Steps:**
```sh
aws sso-admin create-account-assignment --instance-arn arn:aws:sso:::instance/sso-instance-id --target-id aws-account-id --permission-set-arn arn:aws:sso:::permissionSet/permission-set-id --principal-type USER --principal-id user-id
```

---

#### **Multi-Factor Authentication (MFA)**
**Theory:** MFA enhances security by requiring an additional authentication factor.

**UI Steps:**
1. Navigate to IAM Console → "Users".
2. Click "Manage MFA" and enable MFA for the user.
3. Configure a virtual or hardware MFA device.

**CLI Steps:**
```sh
aws iam enable-mfa-device --user-name MyUser --serial-number arn:aws:iam::123456789012:mfa/MyUser --authentication-code-1 123456 --authentication-code-2 789123
```

---

#### **IAM Credential Report**
**Theory:** The IAM Credential Report provides an overview of credentials and security best practices.

**UI Steps:**
1. Navigate to IAM Console → "Credential Report".
2. Click "Download Report".

**CLI Steps:**
```sh
aws iam generate-credential-report
aws iam get-credential-report
```

---

#### **IAM Role Trust Policies**
**Theory:** Trust policies define entities that can assume IAM roles.

**UI Steps:**
1. Navigate to IAM Console → "Roles".
2. Edit the trust relationship of a role.

**CLI Steps:**
```sh
aws iam update-assume-role-policy --role-name MyRole --policy-document file://trust-policy.json
```

---

#### **IAM Security Token Service (STS)**
**Theory:** AWS STS provides temporary, limited-privilege credentials for IAM users or federated users.

**CLI Steps:**
```sh
aws sts assume-role --role-arn arn:aws:iam::account-id:role/MyRole --role-session-name MySession
```

---

#### **Identity Federation and External Identity Providers**
**Theory:** Identity federation allows users from external systems (Google, AD) to access AWS resources.

**UI Steps:**
1. Configure AWS IAM Identity Center (SSO).
2. Set up a trust relationship with the identity provider.

**CLI Steps:**
```sh
aws sso-admin create-trusted-token-issuer --instance-arn arn:aws:sso:::instance/sso-instance-id --name MyIdentityProvider
```

---

#### **IAM Policy Simulator**
**Theory:** The IAM Policy Simulator tests IAM policies before applying them.

**UI Steps:**
1. Open IAM Console → "Policy Simulator".
2. Select a policy and simulate actions.

**CLI Steps:**
```sh
aws iam simulate-principal-policy --policy-source-arn arn:aws:iam::account-id:user/MyUser --action-names s3:ListBucket
```

---

#### **IAM JSON Policy Structure**
**Theory:** IAM policies use JSON format to define permissions.

**Example JSON Policy:**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::my-bucket"
    }
  ]
}
```

