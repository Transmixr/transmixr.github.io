---
title: Live Volumetric Head Avatars
weight: 6
---

The LiveAvatar toolbox converts a monocular portrait video stream, as for example obtained through a laptop webcam, into a 3D volumetric avatar representation of the recorded person in real-time. It uses a pre-trained neural network to generate 3D Gaussian head avatars directly from 2D video, without requiring per-subject setup or fine-tuning.

## Where does it fit in TRANSMIXR?

As an exemplary use case, audience members in the Distributed Studio can join a live broadcast and interact with the presenter and other participants. Users connect to the VR experience from their home devices (laptop, desktop PC, smartphones) that are equipped with a regular RGB front-facing camera. The video stream from their camera is now converted to a 3D avatar and displayed in the virtual studio as a volumetric floating head.

## What problems does it solve?

- Traditional volumetric capture requires multi-camera setups. LiveAvatar generates a 3D outputs from a single 2D camera, making participation accessible to anyone with a webcam.
- Remote participants in XR experiences can be represented as volumetric avatars rather than flat video feeds or generic synthetic avatars

## How it works

Integration of live volumetric head avatars into Unity VR experiences involves four stages:

**Input:** 2D video frames are obtained from the sender device. The toolbox supports reading directly from a connected camera (e.g. webcam), receiving frames via TCP sockets, or receiving frames via WebRTC. Input images are then passed to the LiveAvatar processing server, either locally or on a remote host.

**Processing:** The LiveAvatar server converts input frames using a pre-trained neural network that transforms 2D images into volumetric data (3D RGB point clouds). The process includes image processing, face detection/tracking, and 3D generation. The entire process runs on a frame-by-frame basis without the need for per-subject training or setup. 
Additionally, high-fidelity 2D views can be rendered via Gaussian Splatting from user-defined viewpoints for visualization and 2D use cases. A low-to-mid range GPU is sufficient for processing. An NVIDIA GeForce RTX 2080 Ti achieves frame rates of ca. 20 fps.

**Transmission:** Volumetric data is transmitted as RGB point clouds using [CWIPC](https://github.com/cwi-dis/cwipc) components via TCP. CWIPC supports compressed point cloud transmission as well as additional point cloud processing and visualization functionality. Rendered 2D images can alternatively be streamed as JPEGs via TCP or via WebRTC, allowing easy integration into web-based or mobile applications.

**Rendering:** Volumetric streams produced by LiveAvatar can be integrated into Unity via the [CWIPC Unity package](https://github.com/cwi-dis/cwipc_unity). Please refer to the CWIPC documentation for setup instructions.


## Getting started

**Hardware and Operating System:**

* Python 3.12+, CUDA 12.6+, PyTorch 2.7+
* NVIDIA GPU 
* Ubuntu 24.04 (tested)

**Installation:**

- Download and install CWIPC: [https://github.com/cwi-dis/cwipc](https://github.com/cwi-dis/cwipc)
- Download and install the CWIPC Unity plugin: [https://github.com/cwi-dis/cwipc_unity](https://github.com/cwi-dis/cwipc_unity)
- Download the code from the LiveAvatar repository and follow the installation instructions: [https://github.com/IntelLabs/LiveAvatar](https://github.com/IntelLabs/LiveAvatar)
