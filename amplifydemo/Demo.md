## Amplify Demo

This Demo will build out a simple mobile app using react native and  the aws-mobile cli.  It will be deployed to any phone/simulator using expo and then features will be incrementally added to the app using the aws-amplify mobile library.

### Build the scaffolding

### PreReqs
- Install XCode(Mac) or Android Studio(Win) 
- Install Expo on a Simulator or on your mobile
 - Instructions are found here https://docs.expo.io/versions/latest/introduction/installation.html
 
### Cloud 9 environment
- Navigate to AWS Console -> CloudFormation
- Create New Stack using https://s3-us-west-2.amazonaws.com/cloud9-amplify-demo-sydsummit/CFCloud9Amplify.json
- Next -> Enter Stack Name of Cloud9-amplify-demo Next -> Next-> Create ( wait approx 2-3 mins for Cloud9 Environment to be created)
- Once complete Navigate to Cloud9 -> Open IDE for the ```cloud9-apmlify-demo``` environment
- ```cd Cloud9Amplify/DemoApp```


## Install react and aws tooling
```
nvm install 8
npm i npm@4 -g 
--npm install -g react-native-cli
--npm install -g create-react-native-app
npm install -g awsmobile-cli
npm install -g exp
```
 
## Create react scaffolding
```
create-react-native-app DemoApp
cd DemoApp
```

-----------BaseLined to here -----------

Lets begin by navigating into the prepared react native app, installing the packages and the exp server runtime

```
cd Cloud9Amplify/DemoApp
nvm install 8
npm i npm@4 -g
npm install
npm install -g awsmobile-cli
npm install -g exp
```

Now lets test that all works and we can deploy it to expo running in the simulator

## Test Deployment
```exp start
- UserName: SummitUser/DemoUser!234
```

- Paste Url into Expo App running in simulator
- App should open 
- you should see the following text in the App "Open up App.js to start working..... "
- lets do that -> Open up App.js inside Cloud9 -> Change the text to something else -> Save -> watch it deploy to the simulator ( nice )



## Adding Signin/Signup to your app
Now lets add some signin/signup functionality to our new app - we will use aws amplify and the aws mobile cli to provision the backend.

- Stop the expo server packager 'CTRL-C'
- Initialise the awsmobile_cli that was installed earlier.
```
awsmobile configure
accessKeyId = ////// 
SecretAccessKey = TBD
region = us-west-2
brew
awsmobile init 
- select default values for everything but project name
- Project Name = summit-demo-app
```
This will setup the project and add the aws-amplify packages - NOTE: if there is a yarn error during this step try typing
```
npm install -g aws-amplify
```
This should fix this up and we can now app an API that the App can call...

### add an API to your APP.
Now that we hav e installed Amplify it is easy to build out backend features for your app - lets start with a simple APII
```
awsmobile cloud-api enable
awsmobile push
```

### add user signin
```
npm install -g awsmobile-cli
awsmobile user-signin enable
awsmobile push```

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


