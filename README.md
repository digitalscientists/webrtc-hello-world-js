# WebRTC Hello World

This sample app shows an even simpler way to get a phone to talk to a browser through Bandwidth's WebRTC and Voice APIs, with NodeJS and minimalist browser-side Javascript.

## Architecture Overview

<img src="./WebRTC Hello World.svg">

This app runs an HTTP server that listens for requests from browsers to get connection information. This connection information tells a browser the unique ID it should use to join a WebRTC conference. The HTTP server will also handle requests coming from Bandwidth's Voice API when a phone call comes in.

The server connects to Bandwidth's HTTP WebRTC API, which it will use to create a session and participant IDs. This example leverages our Node SDK to make the WebRTC and Voice API calls.

The web browser will also use a websocket managed by the WebRTC browser SDK to handle signaling to the WebRTC API - this is all handled by a prepackaged Javascript SDK. Once both a browser and a phone have joined the conference, they will be able to talk to each other.

> Note: This is a very simplistic demo and it is not designed to handle multiple browsers or multiple phones.<br/> Unless you are running on `localhost`, you will need to use HTTPS. Most modern browsers require a secure context when accessing cameras and microphones.

## Setting things up

To run this sample, you'll need a Bandwidth phone number, Voice API credentials and WebRTC enabled for your account. Please check with your account manager to ensure you are provisioned for WebRTC.

This sample will need be publicly accessible to the internet in order for Bandwidth API callbacks to work properly. Otherwise you'll need a tool like [ngrok](https://ngrok.com) to provide access from Bandwidth API callbacks to localhost.

```
./ngrok http 3000
```
> Note: This app was hardcoded to run on  `http://localhost:3000`. If you want to run it on a different port, you must change the port in both `server.js` and `public/index.html`.
### Create a Bandwidth Voice API application

Follow the steps in [How to Create a Voice API Application](https://support.bandwidth.com/hc/en-us/articles/360035060934-How-to-Create-a-Voice-API-Application-V2-) to create your Voice API appliation.

In step 7 and 8, make sure they are set to POST.

In step 9, provide the publicly accessible URL of your sample app. You need to add `/incomingCall` to the end of this URL in the Voice Application settings.

You do no need to set a callback user id or password.

Create the application and make note of your _Application ID_. You will provide this in the settings below.

### Configure your sample app

Copy the default configuration files

```bash
cp .env.default .env
```

Add your Bandwidth account settings to `.env`:
- BW_ACCOUNT_ID
- BW_USERNAME
- BW_PASSWORD

Add your Voice API application information:
- BW_VOICE_APPLICATION_ID

Enter your local server address (e.g. ngrok url):
- BASE_CALLBACK_URL

To make an outbound call from the browser, add a phone number to dial:
- USER_NUMBER
- BW_NUMBER (the number that will appear as the FROM for the call)

You can ignore any other settings in the `.env.default` file.

### Install dependencies and build

```bash
npm install
node server.js
```

### Communicate!

Browse to [http://localhost:3000](http://localhost:3000) and grant permission to use your microphone.

- clicking *Get Online* will get a token for your browser, get you connected to our media server, and start media flowing from the browser
- clicking *Dial out* will request the server to start a call out to USER_NUMBER via the Voice API
- clicking *End Call* will request the server to hangup the outbound call

You should now be able to make and receive calls using your browser! The web UI will indicate when you are connected.

Enjoy!