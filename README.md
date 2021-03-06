AWS CodePipeline multiple manual approvals
-----

![](https://codebuild.ap-northeast-1.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiZ21xTkVHQWlqSUJrY21UWUZGSUV4TExYNVVxa3gzcGE5eHlYV1d1clRHY2d0WUJOckNiVWtVUjVzakd1MnBPZVkxV3F1R25iS3NGRkhKdzFKQjIxUGVrPSIsIml2UGFyYW1ldGVyU3BlYyI6IlZQL3ZvakRxaktRRXJSdmsiLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=master)

## Overview

Serverless application that enable multiple manual approvals on AWS CodePipeline.
About [AWS CodePipeline manual approval](https://docs.aws.amazon.com/codepipeline/latest/userguide/approvals.html).

Many thanks to Forrest Brazeal for brilliant idea: [Enforcing the 'Two-Person Rule' with AWS CodePipeline](https://www.trek10.com/blog/enforcing-two-person-rule-aws-codepipeline/).

## Prerequisites

- nodejs8.10
- [yarn](https://yarnpkg.com)
- [aws-cli](https://aws.amazon.com/cli/)
- [aws-sam-cli v0.11.0 or above](https://github.com/awslabs/aws-sam-cli)

## Launch by AWS SAR

The simplest way to launch this application in your account is using [Serverless Application Repository](https://aws.amazon.com/serverless/serverlessrepo/).

[Launch now](https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:461248263104:applications~codepipeline-multi-manual-approvals)

## Setup AWS CodePipeline

AWS CodePipeline supports [AWS Lambda function invoking action](https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-invoke-lambda-function.html).
You should setup a multiple approvals as follows:

1. Add a manual approvals stage

2. Add an action group with multiple manual approval actions

3. Add an `ApprovalChecker` action group below manual approval action group, that invokes the lambda function.

Note that, you **MUST** provide `User parameters` in following format: `{"codepipelinename":"YOUR PIPELINE NAME","approvalstagename":"YOUR MANUAL APPROVAL STAGE NAME AT STEP 1"}`.
So, your could use same function for handler multiple pipelines with difference `User Parameters`.

For examples:
- Assume that your pipeline's name is `ExamplePipeline`

- Create a multiple approvals stage with 2 manual approval actions:

|![create-multiple-approvals-stage.jpg](https://raw.githubusercontent.com/CyberAgent/codepipeline-multi-manual-approvals/master/docs/create-multiple-approvals-stage.jpg)|
|:--:|
|*Multiple approvals stage that named: `Approvals`*|

- `ApprovalChecker` action setting is as follows:

|![create-approval-checker-action.jpg](https://raw.githubusercontent.com/CyberAgent/codepipeline-multi-manual-approvals/master/docs/create-approval-checker-action.jpg)|
|:--:|
|*`ApprovalChecker` action's setting*|

- The `User parameters` is `{"codepipelinename":"ExamplePipeline","approvalstagename":"Approvals"}`

## TODOs

- [ ] Public on Serverless Application Repository in Tokyo region (Please, AWS 🙏)
- [ ] Run locally without real AWS CodePipeline setup

## Contributing

Please, create a pull request and assign to [me](https://github.com/phucnh), I will check as soon as possible.
