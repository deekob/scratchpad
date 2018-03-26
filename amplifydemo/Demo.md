## Amplify Demo

This Demo will build out a simple mobile app using react native and  the aws-mobile cli.  It will be deployed to any phone using expo and then features will be incrementally added to the app using the aws-amplify mobile toolkit.

### Build the scaffolding

### PreReqs
- Install Expo on a Simulator or on your mobile
 - Instructions are found here https://docs.expo.io/versions/latest/introduction/installation.html
 
### Cloud 9 environment
- Navigate to AWS Console -> CloudFormation
- Create New Stack using https://s3-us-west-2.amazonaws.com/cloud9-amplify-demo-sydsummit/CFCloud9Amplify.json
- Next -> Enter Stack Name of Cloud9-amplify-demo Next -> Next-> Create ( wait approx 2-3 mins for CLoud9 Environment to be created)
- Once complete Navigate to Cloud9 -> Open IDE for the ```cloud9-apmlify-demo``` environment
- ```cd Clou9Amplify```


## Install react and aws tooling
```
nvm install 8
npm i npm@4 -g 
npm install -g react-native-cli
npm install -g create-react-native-app
npm install -g awsmobile-cli
npm install -g exp
```
 
## Create react scaffolding
```
create-react-native-app DemoApp
cd DemoApp
```

-----------BaseLined to here -----------


```
cd Cloud9Amplify/DemoAPp
nvm install 8
npm install
npm install -g expo
exp start
- UserName: DemoUser/DemoUser!234
-

## Test Deployment
```sudo sysctl -w fs.inotify.max_user_instances=1024
sudo sysctl -w fs.inotify.max_user_watches=12288
exp start –-tunnel
```
- Paste Url into Expo App running in simulator
- App should open 



## Init awsmobile creds and region
```
awsmobile configure
accessKeyId = ////// 
us-west-2

awsmobile init -- adds amplify 
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


