+++
title = "Tracking Events"
weight = 2
post = ""
+++

The trackers create data on user **actions** at specific points in time. For example:

- First start of the app after installation
- App goes to foreground or background
- App screen is viewed

A number of tracking events are available out of the box. These include, but aren't limited to:

- Screen views
- App lifecycle events
- Installation events

All of these events are enriched with automated session tracking and other device information.

#### Screen View Tracking

Screen views are events tracked when the user views a screen within the application.
In most cases, the trackers automatically track screen view events.
If not tracked automatically, you can still track screen view events manually in your app.

{{< tabs groupId="platform" >}}

{{% tab name="iOS" %}}

Automatic screen view tracking is enabled by default.
It supports UIKit views that are tracked as they appear.

Automatic tracking is not yet available for SwiftUI views.
If using SwiftUI in your application, you can track screen view events manually as the views appear using the [`onAppear` callback](https://developer.apple.com/documentation/charts/chart/onappear(perform:)):

```swift
YourView {
    ...
}
.onAppear {
    let event = ScreenView(name: "home", screenId: UUID())
    Snowplow.defaultTracker()?.track(event)
}
```

{{% /tab %}}
{{% tab name="Android" %}}

Automatic screen view tracking is enabled by default.
It tracks events as `Activity` instances are resumed in their lifecycle.

{{% /tab %}}
{{% tab name="React Native" %}}

The React Native tracker does not yet provide an automated solution to track React Native views.
You can track the screen views manually in the [`useLocation` hooks](https://v5.reactrouter.com/native/api/Hooks/uselocation) in React Router as follows:

```typescript
import React from "react";
import {
  Switch,
  useLocation
} from "react-router-dom";

const useLocationChange = () => {
    const location = useLocation();
    React.useEffect(() => {
        tracker.trackScreenViewEvent({
            id: uuidv4(), // generate a UUID v4 string
            name: 'home',
            type: 'full',
            transitionType: 'none'
        });
    }, [location]);
};

function App() {
  useLocationChange();
  return <Switch>...</Switch>;
}
```

{{% /tab %}}
{{% tab name="Flutter" %}}

The tracker can automatically track view events when currently active pages change through the [Navigator API](https://api.flutter.dev/flutter/widgets/Navigator-class.html).

To activate this feature, one has to register a `SnowplowObserver` retrieved from the tracker instance using `tracker.getObserver()`. The retrieved observer can be added to `navigatorObservers` in `MaterialApp`:

```dart
MaterialApp(
  navigatorObservers: [
    tracker.getObserver()
  ],
  ...
);
```

If using the `Router` API with the `MaterialApp.router` constructor, add the observer to the `observers` of your `Navigator` instance, e.g.:

```dart
Navigator(
  observers: [tracker.getObserver()],
  ...
);
```

Learn more about [screen view autotracking in the Flutter tracker here](https://docs.snowplow.io/docs/collecting-data/collecting-from-own-applications/flutter-tracker/tracking-events/#automatically-tracking-view-events-using-navigator-observer).

{{% /tab %}}
{{< /tabs >}}

#### App Lifecycle Tracking

Application lifecycle state can be captured automatically in some trackers. Once enabled, the tracker will automatically track a [`Background` event](https://docs.snowplow.io/snowplow-android-tracker/classcom_1_1snowplowanalytics_1_1snowplow_1_1event_1_1_background.html) when the app is moved to background and a [`Foreground` event](https://docs.snowplow.io/snowplow-android-tracker/classcom_1_1snowplowanalytics_1_1snowplow_1_1event_1_1_foreground.html) when the app moves back to foreground (becomes visible in the screen). The tracker also attaches a [`LifecycleEntity`](https://docs.snowplow.io/snowplow-android-tracker/classcom_1_1snowplowanalytics_1_1snowplow_1_1entity_1_1_lifecycle_entity.html) to all the events tracked by the tracker reporting if the app was visible (foreground state) when the event was tracked.

{{< tabs groupId="platform" >}}

{{% tab name="iOS" %}}

Enable the `lifecycleAutotracking` flag in `TrackerConfiguration` that is passed to the `Snowplow.createTracker()` call:

```swift
let trackerConfig = TrackerConfiguration()
    .lifecycleAutotracking(true)
```

{{% /tab %}}
{{% tab name="Android" %}}

Enable the `lifecycleAutotracking` flag in `TrackerConfiguration` that is passed to the `Snowplow.createTracker()` call:

```java
TrackerConfiguration trackerConfig = new TrackerConfiguration()
    .lifecycleAutotracking(true);
```

{{% /tab %}}
{{% tab name="React Native" %}}

Enable the `lifecycleAutotracking` flag in `trackerConfig` that is passed to the `Snowplow.createTracker()` call:

```typescript
const tracker = createTracker(
    'ns',
    {
      endpoint: 'https://...',
    },
    {
        trackerConfig: {
            lifecycleAutotracking: true,
        },
    },
);
```

{{% /tab %}}
{{% tab name="Flutter" %}}

The Flutter tracker does not yet support automatic tracking of the application lifecycle.

{{% /tab %}}
{{< /tabs >}}

#### Installation Tracking

It tracks an install event which occurs the first time an application is opened.

{{< tabs groupId="platform" >}}

{{% tab name="iOS" %}}

The installation autotracking is enabled by default.

{{% /tab %}}
{{% tab name="Android" %}}

The installation autotracking is enabled by default.

{{% /tab %}}
{{% tab name="React Native" %}}

The installation autotracking is enabled by default.

{{% /tab %}}
{{% tab name="Flutter" %}}

The Flutter tracker does not yet support automatic tracking of app installation.

{{% /tab %}}
{{< /tabs >}}

#### Custom Event Tracking

Additionally, the tracker enables tracking events with custom structure – called self-describing events.
Self-describing events are based around "self-describing" (self-referential) JSONs, which are a specific kind of [JSON schema](http://json-schema.org/).
A unique schema can be designed for each type of event that you want to track.
This allows you to track the specific things that are important to you, in a way that is defined by you.

A self-describing JSON has two keys, schema and data.
The schema value should point to a valid self-describing JSON schema.
They are called self-describing because the schema will specify the fields allowed in the data value.
Read more about how schemas are used with Snowplow [here](https://docs.snowplowanalytics.com/docs/understanding-tracking-design/understanding-schemas-and-validation/).

{{< tabs groupId="platform" >}}

{{% tab name="iOS" %}}

```swift
let schema = "iglu:com.snowplowanalytics.snowplow/link_click/jsonschema/1-0-1"
let data = ["targetUrl": "http://a-target-url.com"]
let event = SelfDescribing(schema: schema, payload: data)       
tracker.track(event)
```

{{% /tab %}}
{{% tab name="Android" %}}

 ```java
String schema = "iglu:com.snowplowanalytics.snowplow/link_click/jsonschema/1-0-1";
Map data = new HashMap();
data.put("targetUrl", "http://a-target-url.com");
SelfDescribingJson sdj = new SelfDescribingJson(schema, data);
SelfDescribing event = new SelfDescribing(sdj);
tracker.track(event);
```

{{% /tab %}}
{{% tab name="React Native" %}}

```typescript
tracker.trackSelfDescribingEvent({
    schema: 'iglu:com.snowplowanalytics.snowplow/link_click/jsonschema/1-0-1',
    data: {
        targetUrl: 'http://a-target-url.com'
    }
});
```

{{% /tab %}}
{{% tab name="Flutter" %}}

```dart
tracker.track(SelfDescribing(
    schema: 'iglu:com.snowplowanalytics.snowplow/link_click/jsonschema/1-0-1',
    data: {
        'targetUrl': 'http://a-target-url.com'
    }
));
```

{{% /tab %}}
{{< /tabs >}}
