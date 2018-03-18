## Amplify Demo

This Demo will build out a simple mobile app using react native and  the aws-mobile cli.  It will be deployed to any phone using expo and then features will be incrementally added to the app using the aws-amplify mobile toolkit.

### Build the scaffolding

*Install react and aws tooling
```npm install -g react-native-cli
npm install -g create-react-native-app
npm install -g awsmobile-cli
```
 
*Create react scaffolding
```create-react-native-app DemoApp
cd DemoApp
```

*Init awsmobile creds and region
```awsmobile init```
 
*Update the number of file system listeners – expo workaround
```sudo sysctl -w fs.inotify.max_user_instances=1024
sudo sysctl -w fs.inotify.max_user_watches=12288
```
 
*install expo server - ##note: need the tunnel arg for Cloud 9
```1npm install -g exp
exp start –tunnel 
``` 
 
Once Expo server starts it displays a QR-code in the console
Scan QR code with phone app
App is deployed to phone
 
From there is simply a case of add whatever amplify feature you want – for my demo I was going to do these
https://github.com/awslabs/aws-mobile-react-native-starter#use-features-in-your-app


