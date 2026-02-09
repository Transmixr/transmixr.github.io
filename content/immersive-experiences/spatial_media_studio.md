---
title: Spatial Media Studio
weight: 1
---

## Overview

Spatial Media is a distributed broadcast studio based on a client/server
architecture.

### Where does it fit in TRANSMIXR?

The Spatial Media XR Experience package provides tools to broadcast immersive
media in the context of a live TV broadcast on a virtual stage

### What problems does it solve?

1. Display immersive content (360, volumetric)
2. Manage a familiar live broadcast environment for VR
3. Allow spectators to communicate with each other and react live

### How does it work?

The broadcast is transferred over the Internet through two communication
channels: Control (WebSockets) and Data (WebRTC)

Spatial Media is broadly split into five software apps, hereafter referred to
as "roles". A complete broadcast leveraging all features will require all roles
to be up and running. Some roles may require third-party software to be running
alongside them in order to function.

Follow the links below to find instructions to set up and connect each role on
a local network. These instructions assume a simplified topology, where each
role is given to a different machine, and all machines are connected to the
same network.

## Contact

Julien CASTET `<julien.castet@immersion.fr>`

## Components

The Spatial Media Studio is made up of the following components:

1. [Spatial Media Server](/immersive-experiences/server)
2. [Spatial Media Presenter](/immersive-experiences/presenter)
3. [Spatial Media Video Source](/immersive-experiences/videosource)
4. [Spatial Media Director](/immersive-experiences/director)
5. [Spatial Media User](/immersive-experiences/user)
