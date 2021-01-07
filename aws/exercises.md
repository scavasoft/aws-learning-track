# AWS Exercises

### 1. IAM User assuming IAM Role

Create an IAM user with NO permission to create S3 buckets and an IAM role that HAS permission to create S3 buckets. Use
the security credentials of the IAM user to assume the IAM role (using the AWS CLI
and [Profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)) and create a new S3
bucket. Make sure that you get a "permission denied" error whenever you try to create S3 buckets without assuming the
IAM role.

Further reading:

* https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html
* https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html

### 2. API Gateway CRUD operations. Backed by Lambda and DynamoDB

API Gateway + Lambda + DynamoDB - Create an API Gateway that defines a set of standard CRUD API endpoints, where every
endpoint is backed by a Lambda using API Gateway Proxy integration. The Lambdas should read and write to a single Dynamo
DB table. Follow the principle of "least privilege", meaning that no single service should have more privilege than it
actually needs.

References:

* https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-create-api-as-simple-proxy-for-lambda.html
* https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-table-read-write.html

### 3. Lambda -> SNS -> Lambda - Pub/Sub mechanism

Create a Lambda, that, when invoked will create a row in a DynamoDB table "chat_messages" with a unique UUID. The Lambda
will also log the current time within the DynamoDB row, under a field called `message_sent_at=[current_timestamp]`. The
Lambda will also publish the ID of the message to an SNS Topic. On the other side, another Lambda should subscribe to
the SNS topic, and on new messages - find the DynamoDB row that corresponds to this message, and add a new
field `message_received_at=[current_timestamp]`.

References:

* https://docs.aws.amazon.com/sns/latest/dg/sns-lambda-as-subscriber.html
* https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sns-examples-publishing-messages.html
* https://docs.aws.amazon.com/sns/latest/dg/sns-lambda-as-subscriber.html

### 4. Use DynamoDB Streams to create an Audit Log of all changes to any row within a table

Create a DynamoDB table with Stream enabled. Attach a Lambda to that Stream. The purpose of that Lambda is to log all
incoming events in a structured format like `[timestamp][name_of_operation][ID of affected row]` within a predefined
CloudWatch Log Group, the ARN of which is dynamically passed to the Lambda as an Environment Variable (so console.log()
will not help here).

References:

* https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Streams.html

### 5. AppSync -> Lambda -> DynamoDB

Create an AppSync (GraphQL) endpoint with a schema that defines standard CRUD operations that allow you to manage Users within a platform. Every operation should use Lambda as its Data Source. Information should be stored in a DynamoDB table.

References:
* https://aws.amazon.com/appsync/
* https://docs.aws.amazon.com/appsync/latest/devguide/tutorial-lambda-resolvers.html
* https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-table-read-write.html
