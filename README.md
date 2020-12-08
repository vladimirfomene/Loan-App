# Loan App

Mobile App for Loan collection agents

## Prerequisites

To successfully run this app, we need to install the following softwares if we haven't
already done so.

* [Node and NPM](https://nodejs.org/en/download/)
* [NativeScript and Android SDK](https://nativescript-vue.org/en/docs/getting-started/installation/#nativescript-cli)
* 

## Clone Project

Clone the project by issuing the following command in the terminal/console. Once the project is cloned, move to the project's directory with the next command.

```sh
# Clone project
git clone https://github.com/vladimirfomene/Loan-App.git

# move to project directory
cd Loan-App
```
## Install Dependencies

In order to install all the external libraries run the following command:

```sh
# Install dependencies
npm install
```

## Add Backend API URL

Go to the **src** directory of your project and look for the `utils.js` in the 
**utils** subdirectory. Add the public URL you got from *ngrok* to this file like 
so.

```js
export default {
    PHONE_Number: "7777777",
    PASSWORD: "coronavirus",
    API_URL: "http://2e7ae06b05d8.ngrok.io"
}
```

## Install NativeScript Playground and Preview App

Go to the links below to install the apps.

* [NativeScript Playground](https://play.google.com/store/apps/details?id=org.nativescript.play&hl=en&gl=US)
* [NativeScript Preview](https://play.google.com/store/apps/details?id=org.nativescript.preview&hl=en&gl=US)


## Run App

``` sh
# Preview on device
tns preview

# Scan QR Code with Android NativeScript Playground App.

# NativeScript will deploy the app on the Preview App.

# Build for production | platform: android or ios
tns build <platform> --env.production

```
## Agent Login

* **Phone number**: `690346872`
* **Password**: `coronavirus`