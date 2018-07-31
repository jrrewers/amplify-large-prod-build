# NgTest

This project was created to reproduce many files in prouction build issue with AWS Amplify.

Steps to reproduce:
1. Create new project:
ng new someName
cd someName
npm install
npm install aws-amplify-angular --save
npm install aws-amplify --save

2. Create config file
export const gatewayUrl = 'https://sdssdsfsf.execute-api.eu-west-1.amazonaws.com/sdsdsdsdsd/'
export const myEndopint = {
  name: 'myEndopint',
  endpoint: `${gatewayUrl}myEndopint`,
  region: 'eu-west-1'
}
export const awsConfig = {
  Auth: {
    identityPoolId: 'pool_id',
    region: 'eu-west-1',
    userPoolId: 'pool_id',
    userPoolWebClientId: 'webclientid',
  },
  API: {
    endpoints: [myEndopint]
  }
}

3.Initialize Amplify in main.ts
import Amplify from 'aws-amplify';
import {awsConfig} from './aws-config'
Amplify.configure(awsConfig);

4.Modify app.module.ts

import {AmplifyAngularModule, AmplifyService} from 'aws-amplify-angular';
...

@NgModule({
 ...
  imports: [
	...
    AmplifyAngularModule,
  ],
  providers: [
    ...,
    AmplifyService
  ],
})

5. Modify tsconfig.app.json
...
"compilerOptions": {
 ...,
 "types": ["node"]
}

6. Modify polyfills.ts
...
(window as any).global = window;

6. Create prod build
ng build --prod
4 files, about 1.2 MB size are created in dist/

7. Add amplify service to app.component.ts 
import {AmplifyService} from 'aws-amplify-angular';
...

constructor(private amplify: AmplifyService) {}

8. Add amplify-authenticator in app.component.html
...
<amplify-authenticator></amplify-authenticator>

x. Create prod build once again
102 files, about 3 MB size 

Adding service to constructor and using amplify-authenticator causes prod build to create lot of files in prod build and significantly increases build time
