---
title: VR2Gather
layout: home
parent: Components - Creation
nav_order: 5
---

# VR2Gather

VR2Gather is a Unity package to allow creating immersive social VR applications
in Unity. Participants in a VR2Gather-based experience can be represented as
live volumetric video, and see themselves as they are captured live, thereby
allowing more realistic social interaction than in avatar-only based systems.

Experiences created using VR2Gather do not rely on a central cloud-based game
engine. Each participant runs a local copy of the application, and communication
and synchronization is handled through a central experience-agnostic Orchestrator
that handles forwarding of control messages, point cloud streams and
conversational audio between participants.

## Getting Started

### Hardware and Operating System

Currently the best platform for development is Windows 10/11 64 bit, with a
decent GPU. An OpenXR-compatible HMD (Oculus Quest, Vive) is good to have but
not a strict requirement, a lot of interaction can be done (or at least tested)
with keyboard and mouse.

Mac works, with two caveats:

- There is no support for OpenXR (and hence no support for an HMD) in Unity at
  the moment.
- The situation with RGBD cameras on MacOS is a bit of a mess. No problem for
  development, but deploying to Mac is not optimal.

### Software

#### Prerequisites

- The first requirement is Unity. We recommend you install it through the Unity
  Hub. It makes management of different versions easiest
- You need Visual Studio. 2022 is current but 2019 should work. Install the
  Unity development additions. When you install a Unity Editor from the Unity
  Hub it can install VS for you. You want the Unity additions because it makes
  debugging a lot easier
- *Optional:* the software to control your HMD. Probably the Oculus desktop
  software or SteamVR
- You need a working NTP (or other time synchronization implementation) on your
  system. If your system clock is more than about 100 milliseconds off from
  "real time" things will get out of sync
- A VR2Gather orchestrator, running somewhere on a public IP address (or at
  least an IP address reachable by all computers you are going to use for
  VR2Gather). Check the Orchestrator section for more information

#### CWIPC

You probably need the CWI Point Cloud (CWIPC) package. Refer to the CWIPC
section of this site for more information.

If you don't have an RGBD camera you don't need to worry about Intel Realsense
or Microsoft Kinect support, but you still want CWIPC to be able to see
participants that do have a camera.

You will later add the cwipc_unity package to your Unity project, but this
package still requires the cwipc native package to be installed on your machine.

### Create a VR2Gather Project

#### Starting from Scratch

If you are starting from scratch it is easiest to make a copy of our empty
VRTApp-Sample project. You can find it at the top level of the repository. You
can clone the repository using Git or download a ZIP archive from Github. Then,
use Unity Hub to install the corresponding version of Unity and configure the
project.

This project is pre-configured so it already includes the VR2Gather package and
all if its dependencies.

#### Add VR2Gather to an existing Project

If you already have a Unity project and want to add VR2Gather to it:

- Ensure you use the new Input System (not the old Input Manager)
- Add the nl.cwi.dis.cwipc package by Github URL:

      git+https://github.com/cwi-dis/cwipc_unity?path=/nl.cwi.dis.cwipc

- Add our fork of the Unity SocketIO package by itisnajim by Github URL:

      git+https://github.com/troeggla/SocketIOUnity.git#b43e1fa081328eea08f8a7c05c54eba14c97ae22

- Add the nl.cwi.dis.vr2gather.nativelibraries package by Github URL:

      git+https://github.com/cwi-dis/VR2G-nativeLibraries?path=/nl.cwi.dis.vr2gather.nativelibraries

- Add the nl.cwi.dis.vr2gather package by Github URL:

      git+https://github.com/cwi-dis/VR2Gather?path=/nl.cwi.dis.vr2gather

- Add the VR2Gather Essential Assets from Package Manager -> VR2Gather -> Samples
  tab. These are some essential assets, the most important one being the
  LoginManager scene that must be used as the first scene in your application
  (because it creates the shared session).
- Next, in Project Settings, you probably want to add an XR plugin (for example
  OpenXR) and you probably want to add at least one Interaction Profile (for
  example Oculus Touch Controller).
- You may also need to add all the scenes you need to the Build Settings.

You should now be able to run the two "example experiences": Pilot 0 and
Technical Playground. The first one is a minimal space for at most 4 people to
meet. The second one is similar, but it has a few extra objects such as a
mirror and some shared objects such as a "photo camera" that you can use to
take pictures, and mud balls that you can throw at each other.

You will not yet be able to use your own scenes with VR2Gather support: see the
next section for instructions on how to modify your scene and how to include it
as a scenario for VR2Gather.

### Create an Experience

#### Terminology

There are a lot of terms going to be used in this document. So let us start by
explaining them.

- A Scenario is something the participants can experience. It may consist of a
  single scene, but it could have multiple scenes, with all participants going
  from one scene to the next together.
- A Session is a number of participants experiencing a Scenario. One participant
  creates a Session, which is then advertised at the Orchestrator. Other
  participants can then join that Session. At some point in time the creator
  starts the session, and then all participants start with the starting Scene
  of the Scenario.

#### Instructions

- You have followed the steps in the Installation guide and you have a Unity
  project with VR2Gather enabled. Now you want to add your own scenario. There
  are two options:
  - Create a new scene by copying the Pilot0 scene. Subclass any component that
    needs different functionality (for example PilotController) and fix the
    scene to refer to the new component. Add your GameObjects, and remove
    GameObjects you don't need.
  - You already have a scene that works in "normal" Unity. This scene will have
    to be adapted for using VR2Gather. The most important step is that your
    VRRig and interaction GameObjects will have to be changed. The Comparison to
    standard Unity practices document will explain.
- Now you need to create a Scenario. In the LoginManager there is a GameObject
  Tool_ScenarioRegistry with a ScenarioRegistry object. Here you add your
  scenario. The ScenarioID must be globally unique (use a uuid-generator once).
  The ScenarioSceneName is the first Scene used (the one you created in the
  previous step). The Name and Description are for humans only: when the first
  participant creates a session they select this scenario. Other participants
  then see the name and description when they select the session to join it.
- Next you need to ensure your scene is available for loading at runtime. In
  the Build Settings... dialog you can add your scene to the list.
- You can now try your new scenario:
  - Open the LoginManager scene and Play it.
  - Login to the Orchestrator.
  - In the Settings you can set your representation and microphone, this will
    be remembered between runs.
  - Create a session.
  - Select your new scenario from the popup menu.
  - Start the session.
- To try your scenario with two users: The first user follows the steps above,
  the second user uses Join to join the session that the first user created and
  the first user waits until the second user has joined before selecting Start.

You probably need new interactable objects. Create these using one of the
prefabs from the Prefabs section as an example, make sure you copy materials
and subclass any components that need changing).

You can now test these new interactables in SoloPlayground and
TechnicalPlayground, as explained in the Walkthrough section. Then you add them
to your scene.

If you need multiple scenes: your PilotController can open a new scene for you,
and help you ensuring that if one participant goes to the new scene this also
happens for all the other participants.

If you need additional functionality in your P_Player and P_Self_player, for
example if you have multiple avatars that you want to switch between: subclass
the PlayerControllers and provide the new functionality there. Then create
variants of P_Player and P_Player_Self, reference the new controller and add
any GameObjects you need. Finally references these new prefabs in your scene's
SessionPlayersManager.
