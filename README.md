# Cloud9 Demo

   File->New Project with Cloud9 and Lambda 

## Introduction
This demo aims to show users how easy it is to create, build, test and deploy a Lamda function behind an API Gateway EndPoint.  The created Lamda function will be created from one of the existing templates in the [SAM repository]( https://github.com/awslabs/serverless-application-model/tree/master/examples/apps/lambda-canary).  This will be deployed locally in cloud9, where we will debug the deployed application to understand it, change it to suit our needs and then deploy in into AWS.

### Prerequisites
- An AWS account
- [A Cloud 9 environment already set up](https://docs.aws.amazon.com/cloud9/latest/user-guide/setup-express.html)
  - Lamda Roles have been setup
  - Default region has been configured
- Cloud9 
- An open mind...

### Creating a Lambda Function

- Open Cloud9 IDE, Navigate to the "AWS Resources" section on the right hand navigation bar.
  - You will see a Lambda section with Local and Remote Functions
  - We are going to create a Local Function
- Click on the Lamda icon.
  - A dialog will appear "Create servlerless Application"
  - Enter Function Name - "MyFirstFunction" - Application Name Field will mirror this
  - Click Next
  - Select Node.js 6.10 and choose the empty-Nodejs blueprint
  - Clck Next
  - Add an API Gatway trigger with Resource path /isPalindrome
  - For this excercice set the security to none ( just a demo )
  - Next
  - Finish

So now you will have a Function called MyFirstFunction that will be executed when the /todaysWord endpoint it hit.
The editor will open in the index.js of your function - so lets write some code.

**Write or paste in the following code block**
This code is a simple String Palindrome checker - should be pretty evident what its doing
```
exports.handler = (event, context, callback) => {
    // simple function to calculate if passed in event string is Palindromic
    const inputString = event.string;
    const reversyString = inputString.split('').reverse().join('');
    const isPalindrome = (inputString === reversyString);
    
    const result = isPalindrome ? `${inputString} is a Palindrome` : `${inputString} is not a Palindrome`;
    
    callback(null,result);
};
```
### Debuging
Now we have a function lets debug it and test that it works.
- hit the Run Button at the top of the screen (The run/debug panel will appear)
- hit the Run button inside the run panel ( you should see an error - "cannot read property "split" undefined )
  - we havent given a value for 'const string' lets do that
  - add the following Json into the Payload: textarea  ```{"string":"racecar"}```
  - hit the run button again and you should get the output "racecar is a Palindrome"
  - now add a negative test and see that working
  - **Extra Marks** - do the same using breakpoints and changing the value of inputString inside the debugger.
   - tip - make sure you select the debug icon before hitting run.
  
### Deploy and Test

//TODO 



