---
title: XR Gallery
weight: 5
---

## Overview

The XR Gallery is a collaborative XR broadcast control application that
replicates a real world studio in an immersive virtual environment. It allows
Directors, Presenters, and the Interviewees to collaborate in real time, each
through their own version of the application. The system integrates the
volumetric studio, live head avatars, real-time video streaming, and voice
communication.

- **Broadcast control:** Through a VR headset, the Director manages virtual
  cameras and routes media sources to broadcast destinations, with a preview
  step before going live.
- **Volumetric presenter avatars:** Real-time 3D point clouds of presenters’
  heads are streamed into the virtual studio
- **Multi-director collaboration:** Multiple directors can work simultaneously
  with synchronized broadcast pipelines
- **Protocol combination:** NDI, WebRTC, WebSocket, and Vivox protocols work
  together to stream and combine video, audio, and voice into the finalized 2D
  stream

## Where does it fit in TRANSMIXR?

The XR Gallery is part of an experimental research stream in TRANSMIXR. It
provides an immersive XR studio with tools to support the creation of a live TV
broadcast, putting the user into an environment that represents a real world
studio. Its main functionality focuses on media source selection, camera setup,
coordination with presenters, and collaboration between directors.

## What problems does it solve?

1. Broadcast creators are no longer limited to a physical location and can work
   remotely or relocate quickly if needed.
2. All users can be in different locations can collaborate to create a broadcast
   in real time.
3. Integration of volumetric representation of presenters create real time
   face-to-face communication. Especially useful for interviews, reports, or
   podcasts.
4. The Audience has two ways to take part in a broadcast:
   - watch the camera view selected by the Director on a streaming platform
   - or use this application to select one of the virtual camera views themselves, and communicate with the presenters

## How does it work?

The XR Gallery connects users over a network, using a client-server
architecture with Unity Netcode. Different protocols handle connection,
streaming, and routing of data:

- **NDI** transfers video and audio between OBS and Unity, and between machines
  on the same network. The final scene for streaming in OBS is composed out of
  multiple NDI sources. Extensions to NDI plugins were developed to support the
  requirements of the project.
- **WebRTC** streams live broadcast content between Director and Presenter
  computers
- **Vivox** handles voice communication between the stakeholders involved in
  the broadcast.
- **VR2Gather** captures and streams volumetric point cloud data for presenter
  avatars over the local network
