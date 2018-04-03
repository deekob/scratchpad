# Cloud9 Beginner Demo

   Build a simple Lambda Function that sits behind an API gateway endpoint 

## Introduction
This demo aims to show users how easy it is to create, build, test and deploy a Lamda function behind an API Gateway Endpoint.  The Lamda function will be created from one of the existing templates in the [SAM repository]( https://github.com/awslabs/serverless-application-model/tree/master/examples/).  This will initially run locally in Cloud9, where we will debug the application to understand it, change it to suit our needs and then deploy in into AWS.

### Setup
- Navigate to AWS Console -> CloudFormation,
- Create New Stack using https://s3-us-west-2.amazonaws.com/cloud9-debug-demo-sydsummit/CFCloud9Debug.json
- Next -> Enter Stack Name of ```Cloud9-debug-demo``` Next -> Next-> Create ( wait approx 2-3 mins for Cloud9 Environment to be created)

### Creating a Lambda Function

- Open Cloud9 IDE, Navigate to the "AWS Resources" section on the right hand navigation bar.
  - You will see a Lambda section with Local and Remote Functions
  - We are going to create a Local Function
- Click on the Lamda icon.
  - A dialog will appear "Create Servlerless Application"
  - Enter Function Name - "MyFirstFunction" - Application Name Field will mirror this -> Next
  - Select Node.js 6.10 and choose the "empty-Nodejs" blueprint -> Next
  - Add an API Gatway trigger with Resource path /isPalindrome
  - For this excercice set the security to none ( **Only for the demo purposes** The best practices is to configure a security mechanism for your endpoint )
  - Next -> Finish

So now you will have a Function called MyFirstFunction that will be executed when the /isPalindrome endpoint is hit.
The editor will open in the index.js of your function - so lets write some code.

**Write or paste in the following code block**
(replace anything thats already there)

```
exports.handler = (event, context, callback) => {
    // simple function to calculate if passed in string is Palindromic
    const inputString = event.inputWord;
    const reversyString = inputString.split('').reverse().join('');
    const isPalindrome = (inputString === reversyString);
    
    const result = isPalindrome ? `${inputString} is a Palindrome` : `${inputString} is not a Palindrome`;
    
    callback(null,result);
};
```
### Debuging Lambda

Now we have a function, let's run/debug it and test that it works.
- hit the Run Button at the top of the screen (The run/debug panel will appear)
- hit the Run button inside the run panel ( you should see an error - "TypeError: Cannot read property "split" of undefined" ).  This is because we havent passed in a string value for the word we want to test.  Lets do that by using the payload area.
  - add the following Json into the Payload: Textarea  ```{"inputWord":"racecar"}```
  - hit the run button again and you should get the output "racecar is a Palindrome"
  - now add a negative test by changing the payload to test another word and see that working ".... is not a Palindrome"
  - **Extra Marks** - do the same using breakpoints and changing the value of the inputWord inside the debugger.
   - tip - make sure you select the debug icon before hitting run.

Now we have successfuly built and tested the Lambda Paidrome checker - let's now expose it to the world using the API-Gateway endpoint we created earlier

### Debuging Api Gateway Endpoint   

We can also test API gateway endpoint locally - We need to first change our Lambda function so it will work behind an API-Gateway endpoint, by changing how it handles its requests and responses
   - change line 3 of the function to read the queryString Parameters - (we will use query strings for request data - you could also use the body of the request and deserialise the JSON out of that)
   ```
   const inputString = event.queryStringParameters.string;
   ```
   - change the callback method to be Api Gateway friendly
   ```
   callback(null, {
        statusCode: 200,
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(result),
        isBase64Encoded: false
    });
   ```
   - change the run/debug panel so that the API Gateway (local) is the system under test - ( there is a drop down on the RHS of the run/debug button panel.
   - Under Test Payload you should now see entries for Path, Method and Query String
   - Add the following into the Query String field ```inputWord=racecar```
   - See the response is correct and test a few more words to be sure.
   
So we have our Palidrome checker exposed from an API-Gateway endpoint - the last thing we need to do is deploy it into AWS.
  
### Deploy and Test

To Deploy the Function we simply do the following:
- Navigate to the "AWS Resources" section on the right hand navigation bar 
- Right Click on the function and select Deploy
- To test you can either use Cloud9 and the APIGateway (remote) option - or open up the AWS console and test using the API Gateway service Stage URL

Thats it - pretty easy and a great way to Build out your function quickly.

### Extra Marks ## (Optional)

- add some error handling to your function 
- try changing the request to accept JSON payload instead of a query string parameter hand deserialise it
- Inpect the .yml file and see it is completely SAM compliant - try and use SAM on the command line to deploy it.

### Tear Down
- Login to AWS console -> CloudFormation
- Select ```Cloud9-debug-demo``` -> Actions -> Delete Stack





