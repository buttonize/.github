# Buttonize

Hi there! 👋

Buttonize allows you to setup simple UI widgets like buttons or forms with multiple fields and deploy them via your IaC tool of choice, like for example AWS CDK.

## Example

<details>
<summary>Show AWS CDK example code</summary>
  
```typescript
import * as btnz from '@buttonize/cdk'
import * as cdk from 'aws-cdk-lib'
import * as lambda from 'aws-cdk-lib/aws-lambda'
import { Construct } from 'constructs'

export class ExamplesStack extends cdk.Stack {
  constructor(scope: Construct, id: string, props?: cdk.StackProps) {
    super(scope, id, props)

    btnz.GlobalConfig.init(this, {
      apiKey: 'YOUR-BUTTONIZE-API-KEY', // Ideally fetch this information from SSM
      externalId: 'this-is-secret' // Ideally fetch this information from SSM
    })

    const sendUserPasswordResetEmail = new lambda.Function(
      this,
      'SendUserPasswordResetEmail',
      {
        handler: 'index.handler',
        code: lambda.Code.fromInline(`
        exports.handler = async (event) => {
          console.log('Sending email... ')
          return {
            format: 'markdown',
            body: \`
              # Email sent

              
            \`
          }
        };
      `),
        runtime: lambda.Runtime.NODEJS_18_X
      }
    )

    const form = new btnz.Form({
      name: 'Send user password reset email',
      label: 'Send',
      tags: ['prod', 'users']
    })

    form.addTextField('email', {
      label: 'E-mail address of the user',
      placeholder: 'user@example.com'
    })

    form.addTextField('reason', {
      label: 'Internal reason why this action has been performed',
      placeholder: 'hacked',
      regex: '^(hacked|forgot|other)$'
    })

    form.addTextField('note', {
      label: 'Note'
    })

    sendUserPasswordResetEmail.addEventSource(form)
  }
}
```
</details>

**Join our community** [Discord](https://discord.gg/2quY4Vz5BM) | [Twitter](https://twitter.com/SST_dev) | [Website]((https://buttonize.io)) 
