<!-- This is a template you can use for quick progress days. It removes a lot of the steps we encourage you to share in the longer template 000-DAY-ARTICLE-LONG-TEMPLATE.MD-->

![help](/Journey/053/emotion.jpg)

# Cloud Resume Challenge - SAM CLI & CloudFormation

~~It's gone about as well as expected...~~

**UPDATE:** Late on the evening of Christmas Eve I got this working!
![a christmas miracle](/Journey/053/xmas-miracle.png)
It was a combination of syntax (listing each AWS service as an Output in the YAML) and knowing where to implement the CORS permissions. I created a Lambda IAM role with read/write policies, which were then added to the Lambda function. Took a day of Googling to get here, but it's done! Huge thanks to anyone who's shared their Cloud Resume Challenge repos, everyone who has written SAM guides. I know the YAML formatting catches lots of people out, but whatever plugin I've got going on in VS Code (`Prettier?`) has been able to correct most of my indentation errors, and I had very few problems there.

## Cloud Research

- The good: The API endpoint is triggering the Lambda to increment the visitor counter in DynamoDB

- The not-so-good: there's some API Gateway CORS permission problems in the YAML template and the counter won't display client side

![argh](/Journey/053/argh.png)

- The ugly: spent hours deciphering official and non-official documentation. There aren't many video or blog guides out there either

## Social Proof

[Twitter](https://twitter.com/_notwaving/status/1342205028313030657?s=20)
