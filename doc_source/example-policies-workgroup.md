# Workgroup Example Policies<a name="example-policies-workgroup"></a>

This section includes example policies you can use to enable various actions on workgroups\.

A workgroup is an IAM resource managed by Athena\. Therefore, if your workgroup policy uses actions that take `workgroup` as an input, you must specify the workgroup's ARN as follows:

```
"Resource": [arn:aws:athena:<region>:<user-account>:workgroup/<workgroup-name>]
```

Where `<workgroup-name>` is the name of your workgroup\. For example, for workgroup named `test_workgroup`, specify it as a resource as follows:

```
"Resource": ["arn:aws:athena:us-east-1:123456789012:workgroup/test_workgroup"]
```

For a complete list of Amazon Athena actions, see the API action names in the [Amazon Athena API Reference](https://docs.aws.amazon.com/athena/latest/APIReference/)\. For more information about IAM policies, see [Creating Policies with the Visual Editor](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-visual-editor) in the *IAM User Guide*\. For more information about creating IAM policies for workgroups, see [Workgroup IAM Policies](workgroups-iam-policy.md)\.
+ [Example Policy for Full Access to All Workgroups](#example1-full-access-all-wkgs)
+ [Example Policy for Full Access to a Specified Workgroup](#example2-full-access-this-wkg)
+ [Example Policy for Running Queries in a Specified Workgroup](#example3-user-access)
+ [Example Policy for Running Queries in the Primary Workgroup](#example4-run-in-primary-access)
+ [Example Policy for Management Operations on a Specified Workgroup](#example5-manage-wkgs-access)
+ [Example Policy for Listing Workgroups](#example6-list-all-wkgs-access)
+ [Example Policy for Running and Stopping Queries in a Specific Workgroup](#example7-run-queries-access)
+ [Example Policy for Working with Named Queries in a Specific Workgroup](#example8-named-queries-access)

**Example Example Policy for Full Access to All Workgroups**  <a name="example1-full-access-all-wkgs"></a>
The following policy allows full access to all workgroup resources that might exist in the account\. We recommend that you use this policy for those users in your account that must administer and manage workgroups for all other users\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "athena:*"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

**Example Example Policy for Full Access to a Specified Workgroup**  <a name="example2-full-access-this-wkg"></a>
The following policy allows full access to the single specific workgroup resource, named `workgroupA`\. You could use this policy for users with full control over a particular workgroup\.  

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "athena:ListWorkGroups",
                "athena:GetExecutionEngine",
                "athena:GetExecutionEngines",
                "athena:GetNamespace",
                "athena:GetCatalogs",
                "athena:GetNamespaces",
                "athena:GetTables",
                "athena:GetTable"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "athena:StartQueryExecution",
                "athena:GetQueryResults",
                "athena:DeleteNamedQuery",
                "athena:GetNamedQuery",
                "athena:ListQueryExecutions",
                "athena:StopQueryExecution",
                "athena:GetQueryResultsStream",
                "athena:ListNamedQueries",
                "athena:CreateNamedQuery",
                "athena:GetQueryExecution",
                "athena:BatchGetNamedQuery",
                "athena:BatchGetQueryExecution"
            ],
            "Resource": [
                "arn:aws:athena:us-east-1:123456789012:workgroup/workgroupA"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "athena:DeleteWorkGroup",
                "athena:UpdateWorkGroup",
                "athena:GetWorkGroup",
                "athena:CreateWorkGroup"
            ],
            "Resource": [
                "arn:aws:athena:us-east-1:123456789012:workgroup/workgroupA"
            ]
        }
    ]
}
```

**Example Example Policy for Running Queries in a Specified Workgroup**  <a name="example3-user-access"></a>
In the following policy, a user is allowed to run queries in the specified `workgroupA`, and view them\. The user is not allowed to perform management tasks for the workgroup itself, such as updating or deleting it\.   

```
{
    "Version": "2012-10-17",
    "Statement": [
       {
            "Effect": "Allow",
            "Action": [
                "athena:ListWorkGroups",
                "athena:GetExecutionEngine",
                "athena:GetExecutionEngines",
                "athena:GetNamespace",
                "athena:GetCatalogs",
                "athena:GetNamespaces",
                "athena:GetTables",
                "athena:GetTable"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "athena:StartQueryExecution",
                "athena:GetQueryResults",
                "athena:DeleteNamedQuery",
                "athena:GetNamedQuery",
                "athena:ListQueryExecutions",
                "athena:StopQueryExecution",
                "athena:GetQueryResultsStream",
                "athena:ListNamedQueries",
                "athena:CreateNamedQuery",
                "athena:GetQueryExecution",
                "athena:BatchGetNamedQuery",
                "athena:BatchGetQueryExecution", 
                "athena:GetWorkGroup" 
            ],
            "Resource": [
                "arn:aws:athena:us-east-1:123456789012:workgroup/workgroupA"
            ]
        }
    ]
}
```

**Example Example Policy for Running Queries in the Primary Workgroup**  <a name="example4-run-in-primary-access"></a>
In the following example, we use the policy that allows a particular user to run queries in the primary workgroup\.   
We recommend that you add this policy to all users who are otherwise configured to run queries in their designated workgroups\. Adding this policy to their workgroup user policies is useful in case their designated workgroup is deleted or is disabled\. In this case, they can continue running queries in the primary workgroup\.
To allow users in your account to run queries in the primary workgroup, add the following policy to a resource section of the [Example Policy for Running Queries in a Specified Workgroup](#example3-user-access)\.  

```
"arn:aws:athena:us-east-1:123456789012:workgroup/primary"
```

**Example Example Policy for Management Operations on a Specified Workgroup**  <a name="example5-manage-wkgs-access"></a>
In the following policy, a user is allowed to create, delete, obtain details, and update a workgroup `test_workgroup`\.   

```
{
   "Effect": "Allow",
   "Action": [
      "athena:CreateWorkGroup", 
      "athena:GetWorkGroup", 
      "athena:DeleteWorkGroup", 
      "athena:UpdateWorkGroup"
   ],
   "Resource": [
     "arn:aws:athena:us-east-1:123456789012:workgroup/test_workgroup"
   ]
}
```

**Example Example Policy for Listing Workgroups**  <a name="example6-list-all-wkgs-access"></a>
The following policy allows all users to list all workgroups:  

```
{
    "Effect": "Allow",
    "Action": [
        "athena:ListWorkGroups"
    ],
    "Resource": "*"
}
```

**Example Example Policy for Running and Stopping Queries in a Specific Workgroup**  <a name="example7-run-queries-access"></a>
In this policy, a user is allowed to run queries in the workgroup:  

```
{
    "Effect": "Allow",
    "Action": [
        "athena:StartQueryExecution",
        "athena:StopQueryExecution"
    ],
    "Resource": [
        "arn:aws:athena:us-east-1:123456789012:workgroup/test_workgroup"
    ]
}
```

**Example Example Policy for Working with Named Queries in a Specific Workgroup**  <a name="example8-named-queries-access"></a>
In the following policy, a user has permissions to create, delete, and obtain information about named queries in the specified workgroup:  

```
{
    "Effect": "Allow",
    "Action": [
        "athena:CreateNamedQuery",
        "athena:GetNamedQuery",
        "athena:DeleteNamedQuery"
    ],
    "Resource": [
        "arn:aws:athena:us-east-1:123456789012:workgroup/test_workgroup"
    ]
}
```