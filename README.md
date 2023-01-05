# AWS-demo-project
### Create an SST app
Run the following in your working directory.
```sh
$ npx create-sst@latest --template=minimal/javascript-starter notes
$ cd notes
$ npm install
```
By default, our app will be deployed to an environment (or stage) called dev in the us-east-1 AWS region. This can be changed in the ``sst.json`` in your project root.
```HTML
{
  "name": "notes",
  "region": "us-east-1",
  "main": "stacks/index.js"
}
```
Project layout
An SST app is made up of two parts.

```stacks/``` — App Infrastructure

The code that describes the infrastructure of your serverless app is placed in the stacks/ directory of your project. SST uses AWS CDK, to create the infrastructure.

```services/``` — App Code

The Lambda function code that’s run when your API is invoked is placed in the services/ directory of your project.

Later on we’ll be adding a ```frontend/``` directory for our frontend React app.

The starter project that’s created is defining a simple Hello World API. In the next chapter, we’ll be deploying it and running it locally.

### Create a Hello World API

With our newly created SST app, we are ready to deploy a simple Hello World API.

In ```stacks/MyStack.js``` you’ll notice a API definition similar to this.

```sh
import { Api } from "@serverless-stack/resources";

export function MyStack({ stack }) {
  // Create an HTTP API
  const api = new Api(stack, "Api", {
    routes: {
      "GET /": "functions/lambda.handler",
    },
  });

  // Show the endpoint in the output
  stack.addOutputs({
    ApiEndpoint: api.url,
  });
}
```
Here we are creating a simple API with one route, ```GET /.``` When this API is invoked, the function called ```handler``` in ```functions/lambda.js``` will be executed.

Let’s go ahead and create this.

### Starting your dev environment
We’ll do this by starting up our local development environment.

 SST features a Live Lambda Development environment that allows you to work on your serverless apps live.
 ```sh
 npx sst start
 ```
The first time you run this command it’ll take a couple of minutes to deploy your app and a debug stack to power the Live Lambda Development environment.
```sh
===============
 Deploying app
===============

Preparing your SST app
Transpiling source
Linting source
Deploying stacks
dev-notes-MyStack: deploying...

 dev-notes-MyStack


Stack dev-notes-MyStack
  Status: deployed
  Outputs:
    ApiEndpoint: https://guksgkkr4l.execute-api.us-east-1.amazonaws.com

==========================
Starting Live Lambda Dev
==========================

SST Console: https://console.sst.dev/notes/dev/local
Debug session started. Listening for requests...
```
The ApiEndpoint is the API we just created. Let’s test our endpoint. If you open the endpoint URL in your browser, you should see Hello World! being printed out.

<img src="https://imgur.com/CdzTBQm.png" alt="Homepage view 3" width=1000 height=500/>

You can also head over to the SST Console link in your browser. The SST Console is a web based dashboard to manage your SST apps.

<img src="https://imgur.com/u69FwDy.png" alt="Homepage view 3" width=1000 height=500/>

The Local tab shows you real-time logs from your apps.

Note that when you hit this endpoint the Lambda function is being run locally.

## Deploying to prod
To deploy our API to prod, we’ll need to stop our local development environment and run the following.
```sh
npx sst deploy --stage prod
```
We don’t have to do this right now. We’ll be doing it later once we are done working on our app.

The idea here is that we are able to work on separate environments. So when we are working in ```dev```, it doesn’t break the API for our users in ```prod```. The environment (or stage) names in this case are just strings and have no special significance. We could’ve called them ```development``` and ```production``` instead. We are however creating completely new serverless apps when we deploy to a different environment. This is another advantage of the serverless architecture. The infrastructure as code idea means that it’s easy to replicate to new environments. And the pay per use model means that we are not charged for these new environments unless we actually use them.

Now we are ready to create the backend for our notes app. But before that, let’s create a GitHub repo to store our code.
