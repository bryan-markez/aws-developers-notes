# IAM

## IAM JSON Policy Reference

Information taken from [AWS Docs](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html#access_policies-json)

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "1",
            "Effect": "Allow",
            "Principal": { 
                "AWS": [
                    "arn:aws:iam::account-id:root"
                ] 
            },
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::mybucket",
                "arn:aws:s3:::mybucket/*"
            ]
        }
    ]
}
```

**Elements of a JSON Policy**
- "Version"
  - This will almost always be "2012-10-17" except in a few, very rare cases (or if AWS for some reason creates a new version and forces everyone to use it)
- "Statement"
  - Main container for each `Policy Statement` can be a single statement or a list of statements
- "Sid"
  - optional identifier, just helps to organize and clarify what statement does
- "Effect"
  - `Allow` or `Deny`
- "Principal"
  - required if you create a `resource-based policy`
  - indicates which account/user/role to allow/deny access
  - On User or Role based policies, the "Principal" is implied to be that user or role
- "Action"
  - List of actions that the policy statement will allow/deny
- "Resource"
  - required if you create a `IAM-permission policy` (User/Role)
  - Specify a list of resources to which the actions will apply to.
  - On resource-based policies, the "Resource" is implied to be the resource to which this policy was attached to
- "Condition"
  - optional
  - example "Condition":
    - `"Condition" : { "StringEqualsIgnoreCase" : { "aws:username" : "johndoe" }}`
    - the above condition will make sure that only people with the username "johndoe" will get the effect of the policy
  - more information can be found [here](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html)

## Securing IAM users
- Option 1: Password Policy
  - can set minimum password length
  - require specific characters
  - allow/disallow users to change password
  - prevent password re-use
  - require users to change their password after a set time
- Option 2: MFA
  - **Use on every IAM User**
    - At the very least, the Root Account **MUST** be secured with MFA
  - benefit: even if password is stolen, the account is not compromised because most likely the perpetrator does not have the security device.
  - can use either Virtual MFA (Google, Authy) or U2F Security Keys (physical)

## IAM Security Tools
- IAM Credentials Report
  - a report that lists all you accoutn;'s users and the status of their various credentials
- IAM Access Advisor
  - shows the service permissions granted to a user and when thosse services were last accessed

## IAM Best Practices
- Never use Root Account except for AWS setup
- One physical user = One AWS user
- Always try to manage security at a group level, assign users to groups
- Create strong password policy and MFA