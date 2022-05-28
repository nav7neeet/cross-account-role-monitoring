# cross-account-role-monitoring

**Description**<br>
This solution sends an alert notification when cross account access is granted through an IAM role. When a new role is created (event name: CreateRole) or the trust relationship of an existing IAM role is updated (event name: UpdateAssumeRolePolicy) an Amazon Event Bridge rule delivers those specific IAM events to a default event bus in Security Tools account. The default event bus delivers the event to the matching rule which then sends the event object to lambda function for processing. 

 ![image](https://user-images.githubusercontent.com/14819434/170843515-4ca30825-d284-4218-a7c5-6eb3a10be7ef.png)


The lambda function analyses the trust realationship (assume role policy document) of the role to find out cross account access. If cross account access is present, it determines if the AWS account to which cross account access has been granted is internal or external to the AWS Organization. It then publishes an alert message to the SNS topic and a typical alert email looks like this -

![image](https://user-images.githubusercontent.com/14819434/170843518-9e98e2eb-7b27-46a5-99f0-9927b6448fe0.png)


**How to deploy?**<br>
Create an IAM role in the management account to get the list of AWS accounts present in the organization. Lambda function assumes this role get the AWS account list.

Create an Event Bridge rule in all the AWS accounts present in the organization to send specific IAM events (CreateRole, UpdateAssumeRolePolicy) to the default Event Bus in Security Tools account.

Create an Event Bridge rule under default Event Bus in the Security Tools account to send the CreateRole, UpdateAssumeRolePolicy events to Lambda function for processing.


**How to test?**<br>
Create an IAM role with cross account access  or modify an existing role and provide cross account access. You should get an email similar to the one shown above.
