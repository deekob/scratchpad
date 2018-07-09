# Building and Debugging Lamdas with Cloud9

   Build a simple Lambda Function that sits behind an API gateway endpoint, learn how to use the Cloud9 IDE to Build, Debug and Deploy this Function.

## Introduction
This demo aims to show users how easy it is to create, build, test and deploy a Lambda function behind an API Gateway Endpoint using Cloud9.  The function will initially run locally in Cloud9, where we will debug it to gain an  understanding, before changing it to suit our needs, before finally deploying it into AWS.

### Setup - ** Note: resources will be created in us-west-2 **
- Navigate to AWS Console -> Cloud9 -> Account Environments.
- Find the Environment called Cloud9-Debug-Lab and select the "Open IDE" button.
- A new Cloud9 enviroment will open with a few files already checked out from a CodeCommit git repo.
- These Instructions are in that folder structure in a file called README.md - you can view them in markdown by right clicking and selecting 'Preview'. You can then move the resulting tab around in your workspace to wherever makes it most comfortable.


### Creating a Lambda Function
- Once the Cloud9 environment has opened you will see a Folder Structure on the LHS that contains a folder called "isPalindrome" - Hint this is the function you are going to build out. You will also see a bash shell window that we will also use during this lab
- In the bash shell cd into ```~/environment/isPalindrome```
- Select the "AWS Resources" section on the right-hand navigation bar.(it should pop out, if its not already visible)
  - You will see a Lambda section with Local and Remote Functions
  - We are going to create a Local Function
- Click on the Lambda icon at the top.
  - A dialog will appear "Create serverless Application"
  - Enter Function Name - "isPalindrome" - Application Name Field will mirror this -> Next
  - Select Node.js 6.10 and choose the "empty-Nodejs" blueprint -> Next
  - When prompted to add a 'Function trigger' select API Gatway trigger with 'Resource path' /isPalindrome
  - For this excercise set the security to none ( **Only for the demo purposes** The best practices is to configure a security mechanism for your endpoint )
  - Next -> Finish

So now you will have a Function called isPalindrome that will be executed when the /isPalindrome endpoint is hit.
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
### Debugging Lambda

Now we have a function, let's run/debug it and test that it works.
- hit the Run Button at the top of the screen (The run/debug panel will appear)
- hit the Run button inside the run panel ( you should see an error - "TypeError: Cannot read property "split" of undefined" ).  This is because we havent passed in a string value for the word we want to test.  Lets do that by using the payload area.
  - add the following Json into the Payload: TextArea  ```{"inputWord":"racecar"}```
  - hit the run button again and you should get the output "racecar is a Palindrome"
  - now add a negative test by changing the payload to test another word and see that working ".... is not a Palindrome"
  
**adding BreakPoints and using a REPL Loop**
To enable breakpoints, simply make sure the 'debug' icon is enabled beside the run button and you have a breakpoint set in the code in index.js

  - Once you have done this you can hit 'run' and the execution will stop on the breakpoint and a new 'debug' window will open, showing LocalVariables, Call Stack and any watches you have set.
  - Change the value of "inputString" in the 'Local Variables' section and see how that is then reflected when you hit play to continue the execution.
  - **Optional** - Cloud9 also has a REPL (Read-eval-print-loop) which you can access via the Immediate Tab. Try the previous excercise ( i.e changing the InputString value during program execution ) but this time use the Immeduate window when the execution as stopped on your breakpoint.
  - hint - you must be stopped on a breakpoint for the change you make to be applied

Now we have successfully built and tested the Lambda Palindrome checker - let's now expose it to the world using the API-Gateway endpoint we created earlier

### Debugging API-Gateway Endpoint   

We can also test API-Gateway endpoint locally - We need to first change our Lambda function so it will work behind an API-Gateway endpoint, by changing how it handles its requests and responses
   - change line 3 of the function to read the queryString Parameters - (we will use query strings for request data - you could also use the body of the request and deserialise the JSON out of that)
   ```
   const inputString = event.queryStringParameters.inputWord;
   ```
   - change the callback method to be API-Gateway friendly
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
   - save your changes. 
   - change the run/debug panel so that the API-Gateway (local) is the system under test - ( there is a drop down on the RHS of the run/debug button panel - select API Gateway (local) ).
   - Under 'Test Payload' you should now see entries for Function, Path, Method and Query String
   - Add the following into the Query String field ```inputWord=racecar```
   - Hit the 'Run' button after making sure you have removed any breakpoints and the 'Debug' option is disabled
   - See the response is correct and test a few more words to be sure. 
   
So we have our Palindrome checker exposed from a local API-Gateway endpoint - the last thing we need to do is deploy it into AWS.
  
### Deploy and Test

To Deploy the Function we simply do the following:
- Navigate to the "AWS Resources" section on the right hand navigation bar 
- Right Click on the function and select Deploy (This  deploys the Function and the API-Gateway endpoint)
- To test what you have deployed - Navigate back to the AWS Console(outside of CLoud9) -> API Gateway service
- Find the 'cloud9-isPalindrome' service endpoint -> Expand -> Stages -> Expand Stage -> Select GET 
- This should display the Invoke URL
- Copy this and paste into a new Tab in your browser -> append the Query String args so your Get request looks something like:
```https://ind9e8n63f.execute-api.us-west-2.amazonaws.com/Stage/isPalindrome?inputWord=racecar```

That's it - pretty easy and a great way to Build/Test and Deploy functions quickly.

### Extra Marks ## (Optional)

- Inpect the .yml file and see it is completely SAM compliant - try and use SAM on the command line to deploy it.

### Tear Down
- In Cloud9 Navigate to the bash shell
- cd into ```isPalindrome/scripts/``` 
- Execute the cleanup scripts ```./CleanUp.sh```
- Go and do another Lab :)
