# Day 23 (AWS Security - Cloud Enumeration)

* AWS accounts can be accessed programmatically by using an Access Key ID and a Secret Access Key.

* The AWS CLI will look for credentials at ~/.aws/credentials, where you should see:
```txt
aws_access_key_id = <KEY_ID>
aws_secret_access_key = <ACCESS_KEY>
```

* Amazon Security Token Service (STS) allows us to utilise the credentials of a user that we have saved during our AWS CLI configuration.

* We can use the __get-caller-identity__ call to retrieve information about the user we have configured for the AWS CLI:
```sh
aws sts get-caller-identity
```
> {
>     "UserId": "AIDAU2VYTBGYOHNOCJMX3",
>     "Account": "332173347248",
>     "Arn": "arn:aws:iam::332173347248:user/sir.carrotbane"
> }

* Amazon Web Services utilises the Identity and Access Management (IAM) service to manage users and their access to various resources, including the actions that can be performed against those resources.

* IAM has different aspects -that can lead to sensitive data exposure if misconfigured-:
    - Users: each user has a set of credentials, such as passwords or access keys, that can be used to access resources. Furthermore, permissions can be granted at a user level.
    - Groups: can be done to ease the access management for multiple users.
    - Roles: a temporary identity that can be assumed by a user, as well as by services or external accounts, to get certain permissions.
    - Policies: a JSON document that defines access provided to any user, group or role:
        + What action is allowed (Action)
        + On which resources (Resource)
        + Under which conditions (Condition)
        + For whom (Principal)

* Enumerating users:
```sh
aws iam list-users
```

* Policies can be inline or attached:
    - __Inline policies__ are assigned directly in the user (or group/role) profile and hence will be deleted if the identity is deleted. These can be considered as hard-coded policies.
    - __Attached policies__, also called managed policies, can be considered reusable. Every identity that policy is attached to will inherit the policy change automatically.

* Enumerating user policies:
    - inline policies:
    ```sh
    aws iam list-user-policies --user-name USERNAME
    ```
    - attached policies:
    ```sh
    aws iam list-attached-user-policies --user-name USERNAME
    ```
    - groups:
    ```sh
    aws iam list-groups-for-user --user-name USERNAME
    ```

* Checking a user policy -known to be existing-:
```sh
aws iam get-user-policy --policy-name POLICYNAME --user-name USERNAME
```

* Enumerating Roles:
```sh
aws iam list-roles
```

* Enumerating role policies:
    - inline policies:
    ```sh
    aws iam list-role-policies --role-name bucketmaster
    ```
    - attached policies:
    ```sh
    aws iam list-attached-role-policies --role-name bucketmaster
    ```

* permissions provided by a policy:
```sh
aws iam get-role-policy --role-name bucketmaster --policy-name BucketMasterPolicy
```

* Obtaining the temporary credentials which enable assuming a role:
    - Example:
    ```sh
    aws sts assume-role --role-arn arn:aws:iam::123456789012:role/bucketmaster --role-session-name TBFC
    ```
    - generates a temporary set of credentials to assume the bucketmaster role. The temporary credentials will be referenced by the session-name "TBFC"
    - The output will provide us the credentials we need to assume this role.

* Setting the Temporary Credentials to Assume Role:

```sh
# using obtained credentials
export AWS_ACCESS_KEY_ID="<KEY_ID>"
export AWS_SECRET_ACCESS_KEY="<ACCESS_KEY>"
export AWS_SESSION_TOKEN="<TOKEN>"
```

* Checking if correctly assumed a role:
```sh
aws sts get-caller-identity
# it should specify the assumed role
```
    
## S3 (Simple Storage Service)

* S3 is an object storage service provided by Amazon Web Services that can store any type of object such as images, documents, logs and backup files.

* Companies often use S3 to store data for various reasons:
    - reference images for their website.
    - documents to be shared with clients.
    - files used by internal services for internal processing.

* Any object you store in S3 will be put into a "Bucket". You can think of a bucket as a directory where you can store files, but in the cloud.

* Listing Contents From a Bucket:
```sh
aws s3api list-buckets
aws s3api list-objects --bucket BUCKET_NAME
aws s3api get-object --bucket BUCKET_NAME --key OBJECT_NAME OUTPUT_NAME
```