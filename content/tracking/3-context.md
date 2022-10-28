+++
title = "Adding Context"
weight = 3
+++

Whilst the tracking set-up provides event data on user **actions** at a specific point in time, **context** describes the setting in which an event takes place.
To describe the context of an event, we need to define and capture individual **entities**.
For example:

- The user performing an action
- The mobile device the action occured on
- A product that has been interacted with

Together, these entities make up the context of an event.

The mobile trackers add a number of context entities to events by default.
Unless disabled in the tracker configuration, the following entities will be attached to your events:

| Entity | Description |
|---|---|
| Screen | Information about the current screen being viewed when an event occurs |
| Application | Version number and build name of the application |
| Session | Information about the user session |
| Platform | Information about the user device and OS |

{{% notice note %}}
You can also track custom context entities with some or all of your events. To learn more about tracking custom context entities, see the docs for [Android, iOS](https://docs.snowplow.io/docs/collecting-data/collecting-from-own-applications/mobile-trackers/custom-tracking-using-schemas/#tracking-a-custom-entity), [React Native](https://docs.snowplow.io/docs/collecting-data/collecting-from-own-applications/react-native-tracker/tracking-events/#custom-event-contexts), or [Flutter](https://docs.snowplow.io/docs/collecting-data/collecting-from-own-applications/flutter-tracker/adding-data/#event-context)
{{% /notice %}}
