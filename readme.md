In AWS Lambda authorizers for API Gateway, the term "principal" typically refers to the entity or identity associated with the incoming request. This is the entity that is authenticated and authorized to access the API. The principal is often represented by a user, an application, or another entity that can be identified.

When you configure a Lambda authorizer for an API Gateway, the Lambda function you provide is responsible for validating the incoming request's token or credentials and determining the associated principal.

The principal returned by the Lambda authorizer is often used in conjunction with IAM policies to determine what actions the principal is allowed to perform on the AWS resources. The principal can be any identifier that you choose, such as a user ID or an application name.

Here's a simplified example of a Lambda authorizer function in Python that returns a principal:

```python
import json

def lambda_handler(event, context):
    # Extract the token or credentials from the incoming request
    authorization_token = event['authorizationToken']

    # Perform validation and authentication logic
    # ...

    # Determine the principal based on the validated token or credentials
    principal_id = "user123"  # Replace with your actual principal ID

    # Return the policy document along with the principal
    policy_document = generate_policy(principal_id, "Allow", event['methodArn'])
    
    return {
        'principalId': principal_id,
        'policyDocument': policy_document
    }

def generate_policy(principal_id, effect, resource):
    return {
        'Version': '2012-10-17',
        'Statement': [{
            'Action': 'execute-api:Invoke',
            'Effect': effect,
            'Resource': resource
        }]
    }
```

In this example, `principal_id` is a placeholder for the identifier associated with the authenticated entity. The `generate_policy` function creates a basic IAM policy that allows the principal to invoke the API Gateway resource specified in the `resource` parameter.

Remember to adapt the code to your specific authentication and authorization logic, as well as the structure of your API Gateway and the AWS resources you are protecting.
