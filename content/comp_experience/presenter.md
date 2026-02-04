---
title: Spatial Media Presenter
layout: home
parent: Immersive Experience
nav_order: 2
---

The Presenter is the role responsible for acquiring audio and volumetric visuals representing the news anchor on stage.

### Install
The recommended way to install the Presenter role is through the provided Presenter Setup program. This tutorial will assume that you have installed all optional features provided by Presenter Setup.

<details>

<summary><i>Features provided by Presenter Setup</i></summary>

Feature  | Description
---------|---------------------
CWI Point Cloud Toolkit | Installs the Point Cloud toolkit by CWI, required to obtain the volumetric data sent over the network.

</details>

### Startup
The Spatial Media Presenter app can be started from its provided shortcut on the Start Menu.

First, start the volumetric capture source using the `cwipc_forward` command, specifying a port number using the `--port` option.

Next, launch Spatial Media Presenter, and enter the IP address of the Server machine into the Network prompt. Two boxes, labeled "Volumetric" and "Microphone" should now appear.

Into the Volumetric field, enter the URL to the capture source, for instance `tcp://127.0.0.1:PORT` where `PORT` is replaced with the port number you chose previously.

The "Presenter" light on Spatial Media Server will only indicate connection status once the volumetric display has been enabled, through either the OSC Dashboard or the Director role.
