+++
title = "Testing"
weight = 3
+++

We will be using [Snowplow Micro](https://docs.snowplow.io/docs/understanding-your-pipeline/what-is-snowplow-micro/) as a small replacement of a Snowplow pipeline for local testing.
Snowplow Micro is a local Web server that lets you inspect tracked events.

#### **Step 1:** Start Micro using Docker

You will need to have Docker installed.
See the [instructions here](https://docs.docker.com/get-docker/) in case you don't have it installed yet.

The following command can be used to run Micro using Docker:

```sh
docker run -p 9090:9090 snowplow/snowplow-micro:1.3.3
```

#### **Step 2:** Install and start ngrok

Additionally, you will need the Micro server to be accessible to your mobile devices or emulator VMs.
The easiest way to achieve this is through the [ngrok proxy](https://ngrok.com/) that creates a publicly accessible URL for your local server.
You can install ngrok on macOS using the following command ([see here](https://ngrok.com/download) for installation options on other platforms):

```sh
brew install ngrok/ngrok/ngrok
```

You will need to [sign up](https://dashboard.ngrok.com/signup) on the ngrok website and get an authtoken that you can then add to your ngrok installation using:

```sh
ngrok config add-authtoken <token>
```

Finally, you can start ngrok as follows:

```sh
ngrok http 9090
```

This will start the proxy and show you the "Forwarding" URL at which your Micro will be accessible (it has the format `https://XXXX-XX-XX-XXX-XX.ngrok.io`)

#### **Step 3:** Configure the endpoint in your app

Enter the forwarding URL in the `createTracker` call in your app – see [step 3 here](../1-setup#step-3-configure-the-tracker).
Build and run your app in an iOS simulator, Android emulator, or an actual device and use the app so that a number of events are tracked.

#### **Step 4:** Check your data

Navigate to [http://localhost:9090/micro/all](http://localhost:9090/micro/all) on your computer to see the number of tracked events.
You should see a JSON with the counts of total, good, and bad events.
Good events are events that passed validation while bad events resulted in validation errors when checking against their expected schema.

![Micro all events](../images/micro-all.png)

To inspect the good events in more detail, navigate to [http://localhost:9090/micro/good](http://localhost:9090/micro/good).

![Micro good events](../images/micro-good.png)

In case there are bad events, you can inspect their validation errors at [http://localhost:9090/micro/bad](http://localhost:9090/micro/bad).
