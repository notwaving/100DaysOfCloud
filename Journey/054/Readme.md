# Cloud Resume Challenge - Testing the Lambda Function

## Introduction

Only a couple of requirements left on the list. Thought I'd get the tests working before wrapping everything up in a YAML template and implementing a GitHub Action.

## Prerequisite

- Python syntax
- Some knowledge of testing
- [This](https://realpython.com/python-testing/) is a really nicely written guide

## Use Case

- Before we create a CI/CD pipeline for the backend, we know that AWS handles/tests the services as described in the YAML template. However, in a production scale version of an app - with developers potentially making changes to the Lambda function - we'd want testing for the Python code that powers it.

## Cloud Research

- Read the guide, linked in _Prerequisite_ above
- Imported boto3, os, pytest, moto and the lambda_function itself, although that's come with problems because the folder is called `lambda`. This is reserved keyword in Python, so won't work as intended, but changing the folder name breaks the build... Next steps would be to

## Next Steps

`Build Failed > Error: PythonPipBuilder:ResolveDependencies`
Find out where this error is coming from exactly, so we can rename the folder currently called `lambda`.

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1342815330222149632?s=20)
