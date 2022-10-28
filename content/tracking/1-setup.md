+++
title = "Setup Your Tracking"
weight = 1
post = ""
+++

To instrument tracking, you will need to install and configure the tracker package in your mobile app.

#### Step 1: Install the tracker package

{{< tabs groupId="platform" >}}

{{% tab name="iOS" %}}

You can install the tracker using SPM as follows:  
1. In Xcode, select File > Swift Packages > Add Package Dependency.
2. Add the URL where to download the library: https://github.com/snowplow/snowplow-objc-tracker

To learn about other options for installing the tracker (e.g. using CocoaPods or Carthage), [see the mobile tracker documentation](https://docs.snowplow.io/docs/collecting-data/collecting-from-own-applications/mobile-trackers/installation-and-set-up/).

{{% /tab %}}
{{% tab name="Android" %}}

The tracker can be installed using Gradle. Add the following to your `build.gradle` file:

```
dependencies {
  ...
  // Snowplow Android Tracker
  implementation 'com.snowplowanalytics:snowplow-android-tracker:3.+'
  // In case 'lifecycleAutotracking' is enabled
  implementation 'androidx.lifecycle-extensions:2.2.+'
  ...
}
```

{{% /tab %}}
{{% tab name="React Native" %}}

To install the tracker, add it as a dependency to your React Native app:

```bash
npm install --save @snowplow/react-native-tracker
```

{{% /tab %}}
{{% tab name="Flutter" %}}

Add the Snowplow tracker as a dependency to your Flutter application:

```bash
flutter pub add snowplow_tracker
```

{{% /tab %}}
{{< /tabs >}}

#### Step 2: Import the tracker package

{{< tabs groupId="platform" >}}

{{% tab name="iOS" %}}

We recommend creating a class or a file with helper functions (e.g., `Tracker.swift`) to manage all the Snowplow related code in your project.
Import the Snowplow tracker package in the file as follows:

```swift
import SnowplowTracker
```

{{% /tab %}}
{{% tab name="Android" %}}

We recommend creating a class or a file with helper functions (e.g., `Tracker.kt`) to manage all the Snowplow related code in your project.
Import the Snowplow tracker package in the file as follows:

 ```java
import com.snowplowanalytics.snowplow.Snowplow;
import com.snowplowanalytics.snowplow.configuration.NetworkConfiguration;
import com.snowplowanalytics.snowplow.configuration.TrackerConfiguration;
import com.snowplowanalytics.snowplow.controller.TrackerController;
```

{{% /tab %}}
{{% tab name="React Native" %}}

We recommend creating a class or a file with helper functions (e.g., `tracker.js`) to manage all the Snowplow related code in your project.
Import the Snowplow tracker package in the file as follows:

```typescript
import { createTracker } from '@snowplow/react-native-tracker';
```

{{% /tab %}}
{{% tab name="Flutter" %}}

We recommend creating a class or a file with helper functions (e.g., `tracker.dart`) to manage all the Snowplow related code in your project.
Import the Snowplow tracker package in the file as follows:

```dart
import 'package:snowplow_tracker/snowplow_tracker.dart'
```

{{% /tab %}}

{{< /tabs >}}

#### Step 3: Configure the tracker

The next step is to initialize the tracker instances in your app.
Tracker instances are initialized given configuration that includes network settings, tracker feature settings, session settings, and more.

The following snippets show how to initialize tracker instances using the default settings.
They call the `createTracker` function and with some arguments:

1. The tracker namespace which uniquely identifies the tracker within the app (`ns` in the snippet).
2. Network configuration with the endpoint address of the Snowplow Collector (e.g. [Snowplow Micro](https://docs.snowplowanalytics.com/docs/understanding-your-pipeline/what-is-snowplow-micro/) or [Snowplow Mini](https://docs.snowplowanalytics.com/docs/understanding-your-pipeline/what-is-snowplow-mini/)) to send events to. We will update this with a testing endpoint later when we [discuss the testing setup](../4-testing).
3. Tracker configuration with the application ID that identifies events across different applications (`appId` in the snippet).

{{< tabs groupId="platform" >}}

{{% tab name="iOS" %}}

```swift
let networkConfig = NetworkConfiguration(endpoint: "{{URL for Collector}}")
let trackerConfig = TrackerConfiguration()
    .appId("appId")
let tracker = Snowplow.createTracker(
    namespace: "ns",
    network: networkConfig,
    configurations: [trackerConfig]
)
```

{{% /tab %}}
{{% tab name="Android" %}}

 ```java
NetworkConfiguration networkConfig = new NetworkConfiguration("{{URL for Collector}}");
TrackerConfiguration trackerConfig = new TrackerConfiguration("appId");
TrackerController tracker = Snowplow.createTracker(context,
    "ns",
    networkConfig,
    trackerConfig
);
```

{{% /tab %}}
{{% tab name="React Native" %}}

```typescript
const tracker = createTracker(
    'ns',
    {
      endpoint: '{{URL for Collector}}',
    },
    {
        trackerConfig: {
            appId: 'appId',
        },
    }
);
```

{{% /tab %}}
{{% tab name="Flutter" %}}

```dart
SnowplowTracker tracker = await Snowplow.createTracker(
    namespace: 'ns',
    endpoint: '{{URL for Collector}}',
    trackerConfig: const TrackerConfiguration(
      appId: 'appId')
);
```

{{% /tab %}}
{{< /tabs >}}
