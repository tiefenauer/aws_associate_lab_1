# Lab 1

- TODO 1/Step 12: I verified my role has sufficient permission: `aws sts get-caller-identity --profile developer`
- Step 16: I listed all S3 buckets for my account: `aws s3 ls --profile developer`
- Step 17: I tried removing a bucket using AWS CLIresulting in an error (permission denied): 
  ```
  $ aws s3 rb s3://qls-20540-46fe940b4b4ce45a-lab1deletemebucket-18odi0w2un932 --profile developer
  ``` 
- Step 19: I debugged the request to find the reason:

```bash
$ aws s3 rb s3://qls-20540-46fe940b4b4ce45a-lab1deletemebucket-18odi0w2un932 --profile developer --debug
# break down request into individual parts
2022-04-25 12:59:13,152 - MainThread - awscli.clidriver - DEBUG - CLI version: aws-cli/2.5.8 Python/3.9.11 Windows/10 exec-env/EC2 exe/AMD64
2022-04-25 12:59:13,152 - MainThread - awscli.clidriver - DEBUG - Arguments entered to CLI: ['s3', 'rb', 's3://qls-20540-46fe940b4b4ce45a-lab1deletemebucket-18odi0w2un932', '--profile', 'developer', '--debug']
(...)
# sets config variable and skips environment variable
2022-04-25 12:59:13,199 - MainThread - botocore.session - DEBUG - Setting config variable for profile to 'developer'
(...)
2022-04-25 12:59:13,214 - MainThread - botocore.credentials - DEBUG - Skipping environment variable credential check because profile name was explicitly set.
(...)
# begins authentication process
2022-04-25 12:59:13,214 - MainThread - botocore.credentials - DEBUG - Looking for credentials via: assume-role
2022-04-25 12:59:13,214 - MainThread - botocore.credentials - DEBUG - Looking for credentials via: assume-role-with-web-identity
2022-04-25 12:59:13,214 - MainThread - botocore.credentials - DEBUG - Looking for credentials via: sso
2022-04-25 12:59:13,214 - MainThread - botocore.credentials - DEBUG - Looking for credentials via: shared-credentials-file
2022-04-25 12:59:13,214 - MainThread - botocore.credentials - INFO - Found credentials in shared credentials file: ~/.aws/credentials
(...)
2022-04-25 12:59:13,308 - MainThread - botocore.auth - DEBUG - Signature:
918e766d44b17fffbe5b9f4fc6ab20c8f5a4e9a5be3d5de3be9bfec27d036e91
# connects to Amazon S3 endpoint and gets a 403
2022-04-25 12:59:13,308 - MainThread - botocore.endpoint - DEBUG - Sending http request: <AWSPreparedRequest stream_output=False, method=DELETE, url=https://qls-20540-46fe940b4b4ce45a-lab1deletemebucket-18odi0w2un932.s3.eu-west-2.amazonaws.com/, headers={'User-Agent': b'aws-cli/2.5.8 Python/3.9.11 Windows/10 exec-env/EC2 exe/AMD64 prompt/off command/s3.rb', 'X-Amz-Date': b'20220425T125913Z', 'X-Amz-Security-Token': b'IQoJb3JpZ2luX2VjENX//////////wEaCWV1LXdlc3QtMiJIMEYCIQDafYFLjbmbtEEOJJMhMT0UpNwZkuP/aFZO3RpM9eqb+AIhAOmeLq5kEErHCTGiNCWz16TFc+fGX0exSJo/c9saMOe9KrECCI7//////////wEQAhoMOTkzMzA2NDEyNzIwIgycDjWdG2wfwT+/7b0qhQImZkMKgSZ1Nah4Y6ByJZft3lg3oJ7Db34eoo6nyP4P1umhKUYCbQ6BAbnR1Uoe8GZN+qyBeveWgEJaxDwQCqVX0KE1smJh+tAePqyut+dxfclzPHQh/34vzz0FOmAvE4vcQmiblTTe1yia5vsuV/tj+D5KVT+HXYX2GJVTPPw22dFIP7k6aJVKGckOkbPEeNo/CoZZNN8TYIfPeZXg7W63yOxy8kjVHwUSpa0E7GRmzuRYalYbmjPgPT4Acmy2xPELyxdRSn8ZbprwI7EXlKcfvN133FxiBGdMxix1IZlsBEYApf6WFlZhQZNNsFRuEsOhVjvJOLLEQJbxaHv9UNzSwU8EYZgw3rOakwY6nAHImZt0y6Py89F3d3VL9dw3vzdHhoKP890Kv9rXuTNfy2a61a2QySSaxwGwikCS3LIGJmlhJ+mld5cgLaTbLaqKZ1w29nw28NoH4Fi2s4M9wj++eGl4dojm9GAJSa/MgJVx41hvQBgiWrshQfraBEDTnj+aA44h6mUWakefC+sfKMQrAnvwgHnVfNipuDkAZ9v+OL6LrqHLFq6aD14=', 'X-Amz-Content-SHA256': b'e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855', 'Authorization': b'AWS4-HMAC-SHA256 Credential=ASIA6ORNNA2YLJS7IJVP/20220425/eu-west-2/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date;x-amz-security-token, Signature=918e766d44b17fffbe5b9f4fc6ab20c8f5a4e9a5be3d5de3be9bfec27d036e91', 'Content-Length': '0'}>
2022-04-25 12:59:13,308 - MainThread - botocore.httpsession - DEBUG - Certificate path: C:\Program Files\Amazon\AWSCLIV2\awscli\botocore\cacert.pem
2022-04-25 12:59:13,308 - MainThread - urllib3.connectionpool - DEBUG - Starting new HTTPS connection (1): qls-20540-46fe940b4b4ce45a-lab1deletemebucket-18odi0w2un932.s3.eu-west-2.amazonaws.com:443
2022-04-25 12:59:13,370 - MainThread - urllib3.connectionpool - DEBUG - https://qls-20540-46fe940b4b4ce45a-lab1deletemebucket-18odi0w2un932.s3.eu-west-2.amazonaws.com:443 "DELETE / HTTP/1.1" 403 None
2022-04-25 12:59:13,386 - MainThread - botocore.parsers - DEBUG - Response headers: {'x-amz-request-id': 'DVGQYYWCTC7TS7DN', 'x-amz-id-2': 'de61sYg4h6Afhfq+WLfcoM3TE8GT8enF1mpkCq0ZfawgPj7EYkcPNXgqSuBMrB2TghrUoOrWTQU=', 'Content-Type': 'application/xml', 'Transfer-Encoding': 'chunked', 'Date': 'Mon, 25 Apr 2022 12:59:13 GMT', 'Server': 'AmazonS3'}
2022-04-25 12:59:13,386 - MainThread - botocore.parsers - DEBUG - Response body:
b'<?xml version="1.0" encoding="UTF-8"?>\n<Error><Code>AccessDenied</Code><Message>Access Denied</Message><RequestId>DVGQYYWCTC7TS7DN</RequestId><HostId>de61sYg4h6Afhfq+WLfcoM3TE8GT8enF1mpkCq0ZfawgPj7EYkcPNXgqSuBMrB2TghrUoOrWTQU=</HostId></Error>'
(...)
remove_bucket failed: s3://qls-20540-46fe940b4b4ce45a-lab1deletemebucket-18odi0w2un932 An error occurred (AccessDenied) when calling the DeleteBucket operation: Access Denied

```

- Step 20: list ARNs for the S3-Delete-Bucket-Policy:
  ```
  $ aws iam list-policies --output text --query "Policies[?PolicyName == `S3-Delete-Bucket-Policy`].Arn" --profile developer
  arn:aws:iam::993306412720:policy/S3-Delete-Bucket-Policy
  ```
- Step 22: get policy details for above ARN:
  ``` 
  $ aws iam get-policy-version --policy-arn arn:aws:iam::993306412720:policy/S3-Delete-Bucket-Policy --version-id v1 --profile developer
  PolicyVersion:
  CreateDate: '2022-04-25T12:21:44+00:00'
  Document:
    Statement:
    - Action:
      - s3:DeleteBucket
      Effect: Allow
      Resource: arn:aws:s3:::qls-20540-46fe940b4b4ce45a-lab1deletemebucket-18odi0w2un932
    Version: '2012-10-17'
  IsDefaultVersion: true
  VersionId: v1
  ```
- Step 23: Attach S3-Delete-Bucket-Policy to the notes-application-role:
  ```
  $ aws iam attach-role-policy --policy-arn arn:aws:iam::993306412720:policy/S3-Delete-Bucket-Policy --role-name notes-application-role --profile developer
  (no output)
  ```
  and review the updated policy for the role:
  ```  
  $ aws iam list-attached-role-policies --role-name notes-application-role --profile developer
  AttachedPolicies:
  - PolicyArn: arn:aws:iam::993306412720:policy/qls-20540-46fe940b4b4ce45a-DeveloperLabPolicy-42JIEHMO6GBO
    PolicyName: qls-20540-46fe940b4b4ce45a-DeveloperLabPolicy-42JIEHMO6GBO
  - PolicyArn: arn:aws:iam::993306412720:policy/S3-Delete-Bucket-Policy
    PolicyName: S3-Delete-Bucket-Policy
  - PolicyArn: arn:aws:iam::aws:policy/ReadOnlyAccess
    PolicyName: ReadOnlyAccess
  ``` 
- Step 24: Try to delete again:   
  ```
  $ aws s3 rb s3://qls-20540-46fe940b4b4ce45a-lab1deletemebucket-18odi0w2un932 --profile developer
  remove_bucket: qls-20540-46fe940b4b4ce45a-lab1deletemebucket-18odi0w2un932

  ```
Questions:

- What is an ARN?