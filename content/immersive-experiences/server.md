---
title: Spatial Media Server
weight: 2
indent: true
---

The Server is the only mandatory role, as it keeps track of the broadcast state, and all other roles need to connect to it in order to function.

### Install
The recommended way to install the Server role is through the provided Server Setup program. This tutorial will assume that you have installed all optional features provided by Server Setup.

<details>

<summary><i>Features provided by Server Setup</i></summary>

Feature   | Description
----------|------------------------
CUEZ Automator | Installs the CUEZ Automator app, which sends automation messages to the Server and other machines.
OBS Studio (with recommended settings) | Installs the OBS broadcast suite, as well as the DistroAV plugin for NDI support, and scene presets for a 2D broadcast with virtual cameras.
Open Stage Control (with recommended settings) | Installs Open Stage Control, which can be used to manually send automation messages to the Server, as well as the Spatial Media OSC profile.
Stand-alone Server | Required for this tutorial. Installs the components listed below.
-> IIS Web Server (with recommended settings) | Enables the Microsoft IIS Web Server, and installs required IIS modules for redirecting Websockets.
-> WebRTC Signaling Server | Installs the WebRTC signaling server, which is required to establish a connection between different roles on the network.
NDI Tools     | Installs the NDI Tools program, which can be used to establish a connection to remote NDI equipment on the network.

</details>

### Startup
All apps mentioned below can be started using shortcuts on the Start Menu.

First, start the WebRTC Signaling Server. A terminal window should be displayed, this is where subsequent requests to the signaling server will appear.

Next, launch Spatial Media Server, and check the WebRTC panel on the top left. The Signaling Server light should turn green.

Keep an eye on the Server machine, as Windows Firewall prompts may appear as other roles attempt to connect.
