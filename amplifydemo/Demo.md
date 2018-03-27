## Amplify Demo

This Demo will build out a simple react native mobile app along with the aws mobile hub for the backend.  It will be deployed to any phone/simulator using expo and then features will be incrementally added to the app using the aws-amplify mobile library.

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
- Once the environment is open, a git repo will be cloned inside a terminal window - once this is finished inside the terminal window  ```cd Cloud9Amplify/DemoApp``` - *this directory contains the react native scaffolding for the app we are going to build*


## Environment setup
The first thing we have to do is restore the dependencies into the environment and the tools we are going to use: these tools will be the awsmobile-cli and the expo server. Hint: this is a good chance to test out the copy and paste function of the following block into Cloud9 terminal window.

```
nvm install 8
npm install -g yarn
yarn install
npm install -g awsmobile-cli
npm install -g exp

------

nvm install 8
npm i npm@4 -g
npm install
npm install -g awsmobile-cli
npm install -g exp
```

Now lets test that all works and we can deploy it to expo running in the simulator. Make sure you have a running simulator than 

## Test Deployment
```exp start
Log in with Existing Account
- UserName: SummitUser/Dem!234
```
- Copy the the URL thats in the terminal window into the clipboard.
- Switch to the Phone Simulator(or Phone) with Expo installed -> Click the + at the top right of screen -> Paste Url in.  (If your running from a phone intead of pasting a URL in you can use the QRCode instead 
- App should open 
- You should see the following text in the App "Open up App.js to start working..... "
Lets change this page and see how fast it deploys
 - Open up App.js inside Cloud9 -> Change the text to something else -> Save -> watch it deploy to the simulator ( nice )


### Start Adding Features

Now we have a templated app that we can deploy to the simulator very easily - so lets now plug this into the AWS Mobile Hub and start adding features using aws-amplify. The features we are going to add for this demo is Analystics and API access - however you can also add: Authentication, PubSub, PushNotifications, Storage..


## Add Analytics

- Stop the expo server packager 'CTRL-C'
- Initialise the awsmobile_cli that was installed earlier.
```
awsmobile configure
 - ? accessKeyId = ////// 
 - ? SecretAccessKey = TBD
 - ? region = us-west-2
awsmobile init
 - Accept defaults for src / dist / build command and start command
 - Change the project name to 'summit-demo-app'
```

In the terminal
```
awsmobile analytics enable - (this is enabled by default)
awsmobile push
```
In the App modify App.js and add the following at line 3 , under the imports- 

```
import Amplify, { Analytics } from 'aws-amplify';
import aws_exports from './aws-exports';
Amplify.configure(aws_exports);
Analytics.record('MyEvent');

```

Now lets start the app again in the simulator ```exp start``` -> once its running lets have a look at what the custom analytic we just added looks like. To do this we need to go into the AWS Console -> Mobile Hub -> click on the summit-demo-app -> Analytics ( top RHS of screen ) -> Analytics ( LHS ) -> Events - we can select our 'MyEvent' and see how many times its been hit  
We can change the event and add some attributes - so change the line ```Analytics.record('MyEvent');```
to ```Analytics.record('MyEvent', { location: 'summit', time: 'today' });``` -> save the change an wait for it to be deployed - > then look at the anaytics console again.



## Adding Signin/Signup to your app
Now lets add some signin/signup functionality to our new app - we will use aws amplify and the aws mobile cli to provision the backend.

- Stop the expo server packager 'CTRL-C'



### add an API to your APP.
Now that we hav e installed Amplify it is easy to build out backend features for your app - lets start with a simple APII
```
awsmobile cloud-api enable
awsmobile push
```

### add user signin
```
awsmobile user-signin enable
awsmobile push
```
then

```
import Amplify, { Analytics, Auth } from 'aws-amplify';
import aws_exports from './aws-exports';
Amplify.configure(aws_exports);
```

- check out new user pool

```npm install```  - add missing libs

App.js - change add signin/signup form 
```

 
## Update the number of file system listeners – expo workaround
```

```
 
## install expo server - ##note: need the tunnel arg for Cloud 9
```


```

 
Once Expo server starts it displays a QR-code in the console
Scan QR code with phone app
App is deployed to phone
 
From there is simply a case of add whatever amplify feature you want – for my demo I was going to do these
https://github.com/awslabs/aws-mobile-react-native-starter#use-features-in-your-app


### Tear Down
- Inside Cloud 9 - remove the lambda ?
- Deleoete the cloudformations for mobile hub . (which one ) and amplify-demo 
- remove the mobile hub - :(


