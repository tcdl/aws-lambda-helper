# aws-lambda-helper
Collection of helper methods for lambda

[![Build Status](https://travis-ci.org/numo-labs/aws-lambda-helper.svg?branch=master)](https://travis-ci.org/numo-labs/aws-lambda-helper)
[![codecov.io](https://codecov.io/github/numo-labs/aws-lambda-helper/coverage.svg?branch=master)](https://codecov.io/github/numo-labs/aws-lambda-helper?branch=master)

## Installation
`$ npm install aws-lambda-helper --save`

## Usage

```javascript
  var AwsHelper = require('aws-lambda-helper');

  exports.handler = function(event, context) {
    ...
    //Initialise the helper by passing in the context
    AwsHelper.init(context);
    ...
  }
```

### Invoke a Lamda function

```javascript

  var AwsHelper = require('aws-lambda-helper');

  exports.handler = function(event, context){
    // assume : context.invokedFunctionArn = invokedFunctionArn: 'arn:aws:lambda:eu-west-1:123456789:function:mylambda:prod'

    //Initialise the helper by passing in the context
    AwsHelper.init(context);

    console.log(AwsHelper.env); //prints: prod
    console.log(AwsHelper.region); //prints: eu-west-1
    console.log(AwsHelper.account); //prints: 123456789

    var params = {
        FunctionName: 'MyAmazingLambda',
        Payload: { 'hello': 'world' },
        Qualifier: ''
      };
    AwsHelper.Lambda.invoke(params, function (err, data) {
      AwsHelper.failOnError(err, event, context);
      context.succeed(data);
    });
  }
```

### fn: failOnError

This function will intercept the error if exists and then will call 'context.fail'. 
The purpose of this function is to avoid the need of checking for errors so that in callback you could just focus on the succesfull procedure. 

It can be used as follows: 
```js

 AwsHelper.Lambda.invoke(params, function (err, data) {
      AwsHelper.failOnError(err, event, context);
      context.succeed(data);
  });
  
```

