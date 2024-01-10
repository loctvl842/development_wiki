# AWS

## Congito Commands

**1. Configure AWS access**

- Run the following command then input AWS access and secret key

```sh
aws configure
```

**2. List all user pools**

```sh
aws cognito-idp list-user-pools --max-results 10
```

Sample Output:

```json
{
    "UserPools": [
        {
            "Id": "ap-southeast-1_59HB8huuQ",
            "Name": "ll_pool_superversion-staging",
            "LambdaConfig": {
                "PreSignUp": "arn:aws:lambda:ap-southeast-1:763463813142:function:ll-lambda-dev-cognito-verify-user"
            },
            "LastModifiedDate": 1702354889.009,
            "CreationDate": 1702354889.009
        },
        {
            "Id": "ap-southeast-1_BOGg8kIjj",
            "Name": "amplify_backend_manager_d1rqk1jmsvcstd",
            "LambdaConfig": {
                "CustomMessage": "arn:aws:lambda:ap-southeast-1:7634638413142:function:amplify-login-custom-message-d538965e",
                "DefineAuthChallenge": "arn:aws:lambda:ap-southeast-1:7634638131432:function:amplify-login-define-auth-challenge-d538965e",
                "CreateAuthChallenge": "arn:aws:lambda:ap-southeast-1:7634638131432:function:amplify-login-create-auth-challenge-d538965e",
                "VerifyAuthChallengeResponse": "arn:aws:lambda:ap-southeast-1:7634363813142:function:amplify-login-verify-auth-challenge-d538965e"
            },
            "LastModifiedDate": 1697794099.655,
            "CreationDate": 1697794099.655
        },
        {
            "Id": "ap-southeast-1_eV8wUlsjj",
            "Name": "amplify_backend_manager_d3awaxbezozp3e",
            "LambdaConfig": {
                "CustomMessage": "arn:aws:lambda:ap-southeast-1:7634263813142:function:amplify-login-custom-message-e75fce5a",
                "DefineAuthChallenge": "arn:aws:lambda:ap-southeast-1:7363463813142:function:amplify-login-define-auth-challenge-e75fce5a",
                "CreateAuthChallenge": "arn:aws:lambda:ap-southeast-1:7633463813142:function:amplify-login-create-auth-challenge-e75fce5a",
                "VerifyAuthChallengeResponse": "arn:aws:lambda:ap-southeast-1:7634363813142:function:amplify-login-verify-auth-challenge-e75fce5a"
            },
            "LastModifiedDate": 1697789158.394,
            "CreationDate": 1697789158.394
        },
        {
            "Id": "ap-southeast-1_uNp4EWHli",
            "Name": "ll_pool_staging",
            "LambdaConfig": {
                "PreSignUp": "arn:aws:lambda:ap-southeast-1:7632463813142:function:ll-lambda-dev-cognito-verify-user"
            },
            "LastModifiedDate": 1692851055.978,
            "CreationDate": 1692610696.734
        }
    ]
}
```
