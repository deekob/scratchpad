## Amplify Demo

This Demo will build out a simple react native mobile app using aws mobile hub for the backend.  It will be deployed to a simulator running https://expo.io/ were features will be incrementally built and deployed to the app using the **Aws-Amplify** mobile library.

## Note Full instructions can be found in the code-commit repository https://git-codecommit.us-west-2.amazonaws.com/v1/repos/Summit-AmplifyDemo.  To access this:

- Navigate to AWS Console -> CodeCommit -> Summit-AmplifyDemo (If its not there check the region is us-west-2) -> DemoApp -> Readme.md

### Build the scaffolding

### PreReqs
- Install XCode(Mac) or Android Studio(Win) 
- Install Expo on a Simulator or on your mobile
 - Instructions are found here https://docs.expo.io/versions/latest/introduction/installation.html
 
### Cloud 9 environment
- Navigate to AWS Console -> CloudFormation
- Create New Stack using https://s3-us-west-2.amazonaws.com/cloud9-amplify-demo-sydsummit/CFCloud9Amplify.json
- Next -> Enter Stack Name of ```cloud9-amplify-demo``` Next -> Next-> Create ( wait approx 2-3 mins for Cloud9 Environment to be created)
- Once complete ( around 1 - 2 mins) Navigate to Services -> Cloud9 -> Open IDE for the ```cloud9-apmlify-demo``` environment
- Once the environment is open, a git repo will be cloned inside a terminal window - once this is finished inside the terminal window ```cd Cloud9Amplify/DemoApp``` - **this directory contains the react native scaffolding for the app we are going to build**

## Environment setup
* Note resources will be created in us-west-2 *

The first thing we have to do is restore the dependencies into the environment and the tools we are going to use: these tools will be the awsmobile-cli and the expo server. Hint: this is a good chance to test out the copy and paste function of the following block into Cloud9 terminal window.

```
nvm install 8
npm install -g yarn
yarn install
npm install -g awsmobile-cli
npm install -g exp
```

Now lets test that all works and we can deploy it to expo running in the simulator. Make sure you have a running simulator then run the following in the terminal tab. 

## Test Deployment
```
exp start
Log in with Existing Account
- UserName: SummitUser/Dem!234
```
- Copy the the URL thats in the terminal window into the clipboard.
- Switch to the Phone Simulator with Expo installed -> Click the + at the top right of screen -> Paste Url in.  
- App should open 
- You should see the following text in the App "Welcome: [NAME] to Sydney Summit 2018"
Lets change this page and see how fast it deploys
 - Open up App.js inside Cloud9 -> Replace [NAME] with your name -> Save -> watch it deploy to the simulator

### Start Adding Features

Now we have a templated app that we can deploy to the simulator very easily - lets plug this into the AWS Mobile Hub and start adding features using **aws-amplify**. The features we are going to add for this demo are Analytics and Sign Up/Sign In - however you can also currently add: Cloud-Api, PubSub, PushNotifications and Cloud Storage.

## Adding Analytics and a SignIn Page

- Stop the expo server packager ```CTRL-C```
- Initialise the awsmobile_cli that was installed earlier.
```
awsmobile configure
 - ? accessKeyId = TOODOOO 
 - ? SecretAccessKey = TOOODOOOO
 - ? region = us-west-2

awsmobile init
 - ? Accept defaults for src / dist / build command and start command
 - ? Change the project name to 'summit-demo-app'
```
Now lets enable some Mobile Hub services and code in the neccesary aws-amplify bindings to call them, to do this by simply typing in ```awsmobile features``` and selecting what we need.  For this demo make sure analytics and user-signin are enabled after which perform an ```awsmobile push``` to resync the backend.

Now Modify App.js so the imports and main class look like the following.

```
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import Amplify, { Analytics, Auth } from 'aws-amplify';
import { withAuthenticator, Authenticator } from 'aws-amplify-react-native'
import aws_exports from './aws-exports';
Amplify.configure(aws_exports); 
Analytics.record('myEvent'); //this is a custom event
  
class App extends React.Component {
  render() {
    
    return (
       <Authenticator>
      <View style={styles.container}>
        <Text>Welcome: [NAME] to Sydney Summit 2018</Text> 
      </View>
       </Authenticator>
    );
  }
}

export default withAuthenticator(App)
```
What we have done here is imported the Analytics and Auth components ```import Amplify, { Analytics, Auth } from 'aws-amplify';``` and using a built in higher order component ```withAuthenticator``` we have wrapped or welcome text so its only displayed to an authenticated user ```Authenticator```.

## Test SignIn/SignUP
Now lets start the app again in the simulator ```exp start``` -> once its running you can now see the signIn/SignUp options.  Take it for a spin **Note: It uses MFA so supply a real phone number if you want to login**.  Once you have registered a user login and see the welcome message again.
## Test Analytics
Lets have a look at what the custom analytic we just added looks like. To do this we need to go into the AWS Console -> Mobile Hub -> click on the summit-demo-app -> Analytics ( top RHS of screen ) -> Analytics ( LHS ) -> Events - we can select our 'myEvent' and see how many times its been hit  

## Summary
TODO

### Tear Down
- Run the following from Terminal ```awsmobile delete``` - running this command from the terminal will delete the projects backend
- Login to AWS console -> CloudFormation -> Select Cloud9-amplify-demo -> Actions -> Delete Stack
