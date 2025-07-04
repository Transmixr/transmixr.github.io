---
title: Sense XR
layout: home
parent: Components - Creation
nav_order: 6
---


# Sense XR

**SenseXR** is a multiplatform, lightweight solution for capturing and storing of data from multiple sources in a structured way. These sources can be the game engines the experience is running on, external sensors or additional hardware. 

## Overview

The following section provides an outline on **SenseXR**, focusing on what problems it addresses and how.

### What problems does it solve?

It provides an easy way for developers to capture data for their applications in a way that is structured and easy to analyse, display or extract metrics from. With a focus on being lightweight and assynchronous, **SenseXR** allows for the ongoing capture of multiple streams of data while the user is interacting with the application, without impacting performance, directly from within the game engine. 

Both Unity and Unreal engines are supported as, respectively, a Unity Package and an Unreal Plugin with feature parity between both. In the case of Unreal, it provides Blueprint functions that allow a developer to create an integration without the need of explicit C++ programming skills.

### How does it work?

**SenseXR** is based on [HEAT](https://www.vraisimulation.com/products/heat) by [VRAI](https://www.vraisimulation.com/), adapted for standalone capture and storage of data for the TRANSMIXR project.

It acts as a solution that allows for the capture of data, directly integrated in the Unity and Unreal game engines in open formats (_JSON_ and _CSV_) for two generic types of data, namely:

- **Experience Events** happen during the experience. They may occur more than once, but they consist on actions that have a certain outcome and happened in a specific time and place during the session (e.g.: The user opened a door; an NPC turned on the televison). This way, the _“who”_, _“what”_, _“when”_, _“how”_ of the user and NPC interactions can be captured, so that the _“why”_ can be studied, helping in not only capturing but understanding behaviours. These follow a structure that was inspired by the [xAPI](https://xapi.com/) statements. 
  
  **Experience Events** are captured (pushed) by the developer whenever an event of interest is detected and can be further broken down into two subtypes:
  - **Interaction Events** focus on capturing interactions between participants (real or synthetic) and their actions during the experience. These are the traces of their behaviours and can help build timelines and chains of causalities that provide insights into decision making processes;
  - **Session Events** hold information regarding scene / act transitions as well as overall session duration and outcome (e.g.: “the session was interrupted”, “it concluded”, “successfully/unsuccessfully”). This meta information can be usefull when analysing data to identify certain sessions, or, for instance, to filter out sessions that were too short or didn't finish successfully.

- **Time Series Data**: This is data that is collected periodically (polled). The capturing of this data relies on _Data Collectors_ that poll data sources, such as sensors (heart rate, IMU, GSR) or in-engine (NPC position, Player’s gaze)  at fixed intervals. Data collectors are specified by the developer in terms of what data they capture and the specific interval they should run at. This allows for different sources to have their data polled at a per-source rate (e.g.: Heart Rate at 30Hz, Gaze at 120Hz). Each collector runs in the background in a separate thread.

![folder contents](/docs/assets/images/comp_creation/sense_xr/sample-capture-content.png)

In terms of the data egress, the data is saved locally, in the same device running the experiment. As you can see in the image above, showing the results of a capture session for a sample integration, the resulting capture data has the following characteristics:
- One single `JSON` file exists per session, containing all the collected  _Experience Events_ during the session. If the session is interrupted or aborted, the events that were captured up until that point are saved. The JSON file follows this nomenclature: `"${SessionGUID}_${UserID}_${TimeStamp}.json"`.

- Several `CSV` files per session, containing the Time Series Data. Each Data Collector will output one or several CSV files whenever that collector's number of collected traces per file is exceeded. So, if a collector that has a 1000 traces limit per file ends up collecting 2500 traces, a total of three CSV files for that collector are stored in the respective folder. This allows for the periodic and progressive saving of data on one hand, but also to help alleviate memory as the data is discarded the moment it is stored. The CSV nomenclature follows this format: `${SessionGUID}_${CollectorID}_${TimeStamp}.json`.

## Integration

While the general use-case of SenseXR is for data collection in extender reality projects, it has no dependencies on XR or XR-related libraries.

Currently, **SenseXR supports Unreal 5 (5.3) and Unity 2022.3.X**. 

_Note: Other versions, such as 5.5 and Unity 6000 have been tested, but aren't officially supported._

- For **Unreal**, a `*.uplugin` is provided that can be added to the Engine or Project plugin folders. The plugin also includes sample content (a `*.umap` and a couple of blueprints that demonstrate the data capture mechanisms).

- For **Unity**, a `*.unitypackage` was exported, containing the core assets as well as sample content (a Scene and a couple of MonoBehaviors). 

### Target platforms
 
A subset of the target platforms of the two supported engines can be targeted. Specifically, SenseXR can be used in development of **Android** or **Windows** applications and games via **Unreal 5** and **Unity 2022.3.X**. As the primary focus of SenseXR is the collection of data in mixed reality applications, this covers the more popular options: **Windows for PC-VR and Android for mobile VR experiences**. 

### Sample Integration Projects

Two similar sample integration projects were developed for Unreal and Unity respectively. These showcase how data capture of different types of data can be done in both Unity and Unreal.