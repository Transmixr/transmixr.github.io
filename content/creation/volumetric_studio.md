---
title: Volcap
weight: 2
---
# Volumetric Capture System (Volcap)

**Volcap** is CERTH's VCL3D volumetric capture system, designed with flexibility and extensibility in mind.  
It integrates a wide range of functionalities essential for live and offline volumetric workflows, including:

- **Live streaming** of multi-camera RGB-D data  
- **Real-time point cloud visualization**  
- **Spatial calibration** for multi-camera setups  
- **Temporal synchronization** across devices  
- **On-the-fly 3D reconstruction**  
- **Audio capture and synchronization**  
- **Streaming volumetric data directly into Unity**  
- **Recording and storage of RGB-D data** for offline processing  

Because the volumetric media field is rapidly evolving, Volcap is built with a **modular architecture**.  
This means it can accommodate different camera brands, calibration methods, and reconstruction algorithms, making it highly adaptable to new research directions and industry needs.

---

# Our Role in the EU Horizon2020 Project *TransMIXR*

Within the **TransMIXR project**, our team contributes to two distinct use cases:

## 1. Distribution Studio  
We are responsible for **live-streaming two simultaneously captured and reconstructed people** into a shared Unity environment, complete with synchronized audio.  
The setup supports scenarios such as interviews, where one person acts as a presenter and the other as a guest.

## 2. Performing Arts  
We support the **playback of offline volumetric recordings** inside Unreal Engine.  
This enables pre-captured human performances to be integrated into **mixed-reality theatrical experiences**, blending physical and digital performance elements.

---

# Resources

- **Volcap installation and Unity streaming setup:**  
  - [Release binaries](https://github.com/VCL3D/VolumetricCapture/releases/tag/v2)  
  - [Dual Volcap streaming documentation](https://vcl3d.github.io/VolumetricCapture/docs/v2_dual_volcap/)  

- **Unreal playback setup:**  
  - [Volumetric Capture Video Player](https://defkalionpap.github.io/Volumetric-Capture-Video-Player/)
