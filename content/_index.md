+++
title = "Introduction"
menuTitle = "Introduction"
pre = "<i class='fas fa-rocket'></i> "
chapter = false
weight = 1
+++

!['logo-banner'](images/logo_banner.png)

#### Introduction

Welcome to the **Advanced Analytics for Mobile** accelerator. Once finished, you will be able to build a deeper understanding of customer behaviour on your mobile apps and use your data to influence business decisions.

Here you will learn to:

- Model and Visualise Snowplow data
  - using the [snowplow-mobile](https://hub.getdbt.com/snowplow/snowplow_mobile/latest/) dbt package and Streamlit
  - using our sample data for Snowflake (no need to have a working pipeline)
- Set-up Snowplow Tracking in a mobile app
  - track events both from an native iOS/Android, React Native, or Flutter app
- Apply what you have learned on your own pipeline to gain insights

***

#### System overview

The diagram below gives a complete overview of the system covered in this accelerator:

1. Events are tracked from app logic inside the **mobile app**.
   - Using the [Snowplow iOS](https://github.com/snowplow/snowplow-objc-tracker), [Android](https://github.com/snowplow/snowplow-android-tracker), [React Native](https://github.com/snowplow/snowplow-react-native-tracker), or [Flutter tracker](https://github.com/snowplow-incubator/snowplow-flutter-tracker).
2. Tracked events are loaded into a **Snowflake warehouse** by the Snowplow BDP or Open Source Cloud.
3. The raw events are **modeled into higher level entities** such as screen views, sessions, or users using the [snowplow-mobile](https://docs.snowplowanalytics.com/docs/modeling-your-data/the-snowplow-mobile-data-model/dbt-mobile-data-model/) dbt package.
4. Finally, we **visualize** the modeled data using Streamlit.

{{<mermaid>}}
flowchart TB

subgraph mobileApp[Mobile App]
    appCode[App logic]
    tracker[Snowplow iOS/Android/React Native/Flutter tracker]

    appCode -- "Tracks events" --> tracker

    style tracker fill:#f5f5f5,stroke:#6638B8,stroke-width:3px
    click tracker "https://docs.snowplow.io/docs/collecting-data/collecting-from-own-applications/mobile-trackers/installation-and-set-up/" "Open tracker docs" _blank
end

subgraph cloud[Cloud]
    snowplow[Snowplow BDP/OS Cloud]
    snowflake[(Snowflake)]
    dbt[snowplow-mobile dbt package]
    streamlit[Streamlit]

    snowplow -- "Loads raw events" --> snowflake
    dbt -- "Models data" --> snowflake
    streamlit -- "Visualises modeled data" --> snowflake

    style dbt fill:#f5f5f5,stroke:#6638B8,stroke-width:3px
    click dbt "https://docs.snowplowanalytics.com/docs/modeling-your-data/the-snowplow-mobile-data-model/dbt-mobile-data-model/" "Open dbt package" _blank
    click snowplow "https://snowplow.io/snowplow-bdp/" "Snowplow BDP" _blank
end

tracker -- "Sends tracked events" --> snowplow
{{</mermaid>}}

***

#### Who is this guide for?

- Data practitioners who would like to get familiar with Snowplow data.
- Data practitioners who would like to set up tracking in a mobile app and learn how to use the Snowplow mobile data model to gain insight from their customers' behavioural data as quickly as possible.

***

#### What you will learn

In approximately 10 working hours you can achieve the following:

- **Upload data -** Upload a sample Snowplow events dataset to your Snowflake warehouse
- **Model -** Configure and run the snowplow-mobile data model
- **Visualise -** Visualise the modeled data with Streamlit
- **Track -** Set-up and deploy tracking to your mobile app
- **Next steps -** Gain value from your own pipeline data through modeling and visualisation


{{<mermaid>}}
gantt
        dateFormat  HH-mm
        axisFormat %M
        section 1. Upload
        1h          :upload, 00-00, 1m
        section 2. Model
        2h          :model, after upload, 2m
        section 3. Visualise
        3h          :visualise, after model, 3m
        section 4. Track
        3h          :track, after visualise, 3m
        section 5. Next steps
        1h          :next steps, after track, 1m

{{</mermaid >}}

***

#### Prerequisites

**Modeling and Visualisation**

- dbt CLI installed / dbt Cloud account available
  - New dbt project created and configured
- Python 3 Installed
- Snowflake account and a user with access to create schemas and tables

**Tracking**

- Snowplow pipeline
- Mobile app to implement tracking on

{{% notice info %}}
Please note that Snowflake will be used for illustration but the snowplow-mobile dbt package also supports **BigQuery, Databricks, Postgres** and **Redshift**. Further adapter support for this accelerator coming soon!
{{% /notice %}}

***

#### What you will build

**Advanced Analytics for Mobile Dashboard**

!['logo-banner' ](visualisation/images/streamlit.png?width=100pc)
