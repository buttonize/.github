<p align="center">
  <a href="https://buttonize.io">
    <img width="350" alt="Buttonize.io" src="https://user-images.githubusercontent.com/6282843/212024942-9fd50774-ea26-48ba-b2cf-ca2584498c9a.png">
  </a>
</p>

<p align="center">
  <a href="https://discord.gg/2quY4Vz5BM"><img alt="Discord" src="https://img.shields.io/discord/1038752242238496779?style=flat-square" /></a>
</p>

Hi there! 👋

[Buttonize](https://buttonize.io) is a low-code paltform which enables serverles developers to create UI widgets like buttons, inputs, forms etc. connected to the cloud services like [AWS Lambda](https://aws.amazon.com/lambda/), [AWS Step Functions](https://aws.amazon.com/step-functions/), [AmazonDynamoDB](https://aws.amazon.com/dynamodb/) and more.

## Check out quick use-case demo video

<p align="center">
  <a href="https://youtu.be/giroZqjTJqE"><img width="720" alt="Buttonize use-case demo video" src="https://user-images.githubusercontent.com/6282843/227513455-843d2c82-4d8a-49b8-a324-acb611dcb226.png"></a>
</p>

## Example

<details>
<summary>Show AWS CDK example code</summary>
  
```typescript
import * as path from 'path'
import * as btnz from '@buttonize/cdk'
import * as cdk from 'aws-cdk-lib'
import * as lambda from 'aws-cdk-lib/aws-lambda'
import { NodejsFunction } from 'aws-cdk-lib/aws-lambda-nodejs'
import { Construct } from 'constructs'

export class SimpleFormStack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props)

    btnz.GlobalConfig.init(this, {
      apiKey: cdk.SecretValue.unsafePlainText('YOUR-BUTTONIZE-API-KEY'), // Ideally use SSM or Secrets Manager
      executionRoleExternalId: cdk.SecretValue.unsafePlainText('this-is-secret') // Ideally use SSM or Secrets Manager
    })

    const simpleFormActionLambda = new NodejsFunction(
      this,
      'SimpleFormActionLambda',
      {
        handler: 'handler',
        entry: path.join(__dirname, `/src/index.ts`),
        runtime: lambda.Runtime.NODEJS_18_X
      }
    )

    const form = new btnz.Form({
      name: '[Example: simple-form] Invoke the lambda fucntion',
      label: 'Open form',
      tags: ['simple', 'button', 'example']
    })

    form
      .addTextField('email', {
        label: 'Email of the user',
        placeholder: 'user@example.com',
        regex: '^[\\w-\\.]+@([\\w-]+\\.)+[\\w-]{2,4}$'
      })
      .addToggleField('isAdmin', {
        label: 'Is admin'
      })

    simpleFormActionLambda.addEventSource(form)
  }
}
```
</details>

## Docs

Learn more at [docs.buttnoize.io](https://docs.buttonize.io/infrastructure-as-code/aws-cdk/quick-start)

**Join our community** [Discord](https://discord.gg/2quY4Vz5BM) | [Twitter](https://twitter.com/SST_dev) | [Website](https://buttonize.io)
