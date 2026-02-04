---
title: VR2Gather
weight: 4
---

# VR2Gather

[VR2Gather](https://github.com/cwi-dis/vr2gather) is a Unity package to allow
creating immersive social VR applications in Unity. Participants in a
VR2Gather-based experience can be represented as live volumetric video, and see
themselves as they are captured live, thereby allowing more realistic social
interaction than in avatar-only based systems.

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
- A [VR2Gather Orchestrator](https://github.com/cwi-dis/vr2gather-orchestrator-v2),
  running somewhere on a public IP address (or at least an IP address reachable
  by all computers you are going to use for VR2Gather). Refer to the Orchestrator
  section for more information.

#### Orchestrator

The Orchestrator application is a server to go along with the VR2Gather
architecture, handling user and session management as well as transmission of
binary data between users over the network.

The easiest way to run the orchestrator is through Docker. First, make sure you
have Docker and `docker-compose` installed. Rename the file `.env-sample` to `.env`
and adjust the values as needed. Note that if you are on a UNIX system, this
file is likely hidden.

If you set the key `EXTERNAL_HOSTNAME` in `.env` to `dynamic` or leave it unset
completely, the server will try to determine the external hostname using the
request headers when creating a new session. If it fails to do so, an exception
will be raised.

Next, copy the file `config/config-sample/ntp-config.json` to
`config/ntp-config.json` and adjust as needed. The default value for the key
server, `nl.pool.ntp.org`, should work for most people, but you may want to
choose one geographically closer to you. Also note that the NTP protocol (or
access to public NTP servers) may be blocked inside some corporate intranets.
In this case, contact your IT department and enquire whether they host an
internal NTP server. Otherwise, opt for the `localtime` option described in the
next paragraph.

You can add multiple NTP servers to this config file, they will be tried in
order until the first one returns a valid response. Adding an entry with the
key-value pair `"server": "localtime"` will return the host's local time without
querying an NTP server.

After setting up the config, simply build and start the container by running:

    docker compose up

This will build the container if it hasn't already been built and launch it on
port 8090 (or whatever port you set in `.env`). If this fails, try calling
`docker-compose` instead of `docker compose`. Though this means you are running
an older version of Docker and you should consider upgrading.

The orchestrator can forward binary data (e.g. point clouds) via Socket.IO,
which is also used for all other communication. However, you also have the
option to use an external SFU (Stream Forwarding Unit), as long as it is
supported on the client side. External SFU binaries placed inside a folder
`packages/` in the project root are placed in container during build time into
the folder `/packages`. Keep this in mind when writing SFU config files. The
corresponding config file must be placed into the folder `config/`. See the
file `config/config-sample/webrtc-config.json` as a sample.

*Optional*: install the external tools (the Dash and WebRTC SFUs) into
`config/packages`. There are scripts in `external-packages/` to download and
install the external tools.

#### CWIPC

You probably need the [CWI Point Cloud (CWIPC)](https://github.com/cwi-dis/cwipc)
package.

If you don't have an RGBD camera you don't need to worry about Intel Realsense
or Microsoft Kinect support, but you still want CWIPC to be able to see
participants that do have a camera.

In order to facilitate working with point clouds as opaque objects - similar to
how most software works with images, or audio samples - our group has developed
an open source suite of libraries and tools that we call cwipc (abbreviation of
CWI Point Clouds). The implementation builds on the PCL pointcloud library and
various vendor-specific capturing libraries, but this is transparent to
software using the cwipc suite (but it can access these representations if it
needs to).

The simplest way to install cwipc is through a prebuilt installer. This will
install everything in the standard location, and it allows running the command
line tools as well as developing C, C++, Python or Unity programs that use the
cwipc library.

After installation, run `cwipc_view --synthetic` from a shell (terminal window,
command prompt). It should show you a window with a rotating synthetic point
cloud if everything is installed correctly. There is also a command line
utility cwipc_check that will test that all third-party requirements have been
installed correctly. On Windows you can find these in the start menu too.

- **Windows:** Download the windows installer .exe for the most recent cwipc release
  [here](https://github.com/cwi-dis/cwipc/releases/latest).

  Windows installers often fail because each Windows computer is different.
  Moreover, cwipc depends on some third party packages (such as for Kinect
  support) that we cannot include in our installer because of licensing issues,
  so we have to rely on official installers for those packages.

  After installing, run *Start menu* -> *cwipc* -> *Check cwipc installation*.
  This will open a CMD command window and try to find out if everything has been
  installed correctly. If there are any errors it may show a dialog which mentions
  which library has not been installed correctly. There may be error messages in
  the output window.
- **Linux:** The installer is currently available for Ubuntu 24.04 and 22.04.
  Download the debian package for the most recent cwipc release
  [here](https://github.com/cwi-dis/cwipc/releases/latest).

  Install from the command line with `sudo apt install ./yourpackagename.deb`.

  The Kinect and Realsense SDKs will not be automatically installed, because
  they come from different repositories and not from the standard Ubuntu/Debian
  repositories.
- **macOS:** The installer is available via Homebrew. Install with:

      brew tap cwi-dis/cwipc
      brew install cwipc

  Verify that everything (including the Python packages and scripts) is
  installed correctly by running `cwipc_view --version` from the command line.

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

- Add the VR2Gather Essential Assets from *Package Manager* -> *VR2Gather* -> *Samples*
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

- A **Scenario** is something the participants can experience. It may consist of a
  single scene, but it could have multiple scenes, with all participants going
  from one scene to the next together.
- A **Session** is a number of participants experiencing a Scenario. One participant
  creates a Session, which is then advertised at the Orchestrator. Other
  participants can then join that Session. At some point in time the creator
  starts the session, and then all participants start with the starting Scene
  of the Scenario.

#### Instructions

- You have followed the steps in the Installation guide and you have a Unity
  project with VR2Gather enabled. Now you want to add your own scenario. There
  are two options:
  - Create a new scene by copying the `Pilot0` scene. Subclass any component that
    needs different functionality (for example `PilotController`) and fix the
    scene to refer to the new component. Add your GameObjects, and remove
    GameObjects you don't need.
  - You already have a scene that works in "normal" Unity. This scene will have
    to be adapted for using VR2Gather. The most important step is that your
    `VRRig` and interaction GameObjects will have to be changed.
- Now you need to create a Scenario. In the LoginManager there is a GameObject
  `Tool_ScenarioRegistry` with a `ScenarioRegistry` object. Here you add your
  scenario. The ScenarioID must be globally unique (use a uuid-generator once).
  The ScenarioSceneName is the first Scene used (the one you created in the
  previous step). The Name and Description are for humans only: when the first
  participant creates a session they select this scenario. Other participants
  then see the name and description when they select the session to join it.
- Next you need to ensure your scene is available for loading at runtime. In
  the *Build Settings...* dialog you can add your scene to the list.
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

If you need additional functionality in your `P_Player` and `P_Self_player`, for
example if you have multiple avatars that you want to switch between: subclass
the PlayerControllers and provide the new functionality there. Then create
variants of `P_Player` and `P_Player_Self`, reference the new controller and add
any GameObjects you need. Finally references these new prefabs in your scene's
SessionPlayersManager.

## Setting up your Cameras

Currently, `cwipc` supports Microsoft Kinect Azure and Intel RealSense series
cameras.

Both types are fully supported on Windows. On Linux both types should be
supported, but this has not been tested recently. On Mac only the Realsense
cameras are supported, but there are major issues at the moment (bascially you
have to run everything as root).

You should first register your cameras. This creates a file cameraconfig.json
that will have information on camera serial numbers, where each camera is
located and where it is pointed, and how the captured images of the cameras
overlap. This information is needed to be able to produce a consistent point
cloud from your collection of cameras.

The preferred way to use your cameras is to put them on tripods, in portait
mode, with all cameras having a clear view of the floor of your origin, the
natural "central location" where your subject will be. But see below for
situations where this is not possible.

For registration of the cameras, you need to print the
[origin Aruco marker](https://github.com/cwi-dis/cwipc_util/blob/master/data/target-a4-aruco-0.pdf).
If that link does not work: you can also find the origin marker in your
installation directory, in `share/cwipc/registration/target-a4-aruco-0.pdf`.

Registering your cameras consists of a number of steps:

- Setup your hardware.
- Use `cwipc_register` to find your cameras. This gives you an unaligned
  `cameraconfig.json`.
- Use `cwipc_register` to locate the origin marker in every camera, giving you
  a coarse alignment in `cameraconfig.json`.
- Use `cwipc_register` to do fine alignment.
- Manually edit `cameraconfig.json` to limit the point clouds to the subject
  (removing floor, walls, ceiling, etc).

### cwipc_register

The `cwipc_register` command line utility is the swiss army knife to help you
setup your cameras, but it is rather clunky at the moment. An interactive
GUI-based tool will come at some point in the future.

Use `cwipc_register --help` to see all the command line options it has. For now,
we will explain the most important ones only:

- `cwipc_register` without any arguments will try to do all of the needed steps
  (but it is unlikely to succeed unless you know exactly what you are doing).
- The `--verbose` option will show verbose output, and it will also bring up
  windows to show you the result of every step. Close the window (or press ESC
  with the window active) to proceed with the next step.
- The `--rgb` will use the RGB and Depth images to do the coarse registration
  (instead of the point clouds), if possible. This will give much better results.
- The `--interactive` option will show you the point cloud currently captured.
  You can press w in the point cloud window to use this capture for your
  calibration step. If `--rgb` is also given you will be shown the captured RGB
  data, in a separate window. The `--rgb_cw` and `--rgb_ccw` options can be
  given to rotate the RGB images.
- In `--interactive` mode the `cwipc_register` point cloud window works similar
  to the `cwipc_view` window. So you can use left-mouse-drag to pan around the
  point cloud, right-mouse-drag to move up and down, scrollwheel to zoom. `?`
  will print some limited help on stdout.

So, with all of these together, using `cwipc_register --rgb --interactive` may
allow you to go through the whole procedure in one single step.

### Hardware Setup

Ensure that all cameras are working (using Realsense Viewer or Azure Kinect
Viewer). If you have multiple cameras you are going to need sync cables to
ensure all the shutters fire simultanously for best results. See the camera
documentation. You probably want to disable auto-exposure, auto-whitebalance
and all those.Set those to manual, and in such a way that the colors from all
cameras are as close as possible.

You may need USB3 range extenders (also known as active cables) to be able to
get to all of your cameras. Ensure these work within the camera viewer.

Put your origin marker on the floor and ensure all cameras can see it in RGB
and Depth. The latter may be a bit difficult (because you can't see the marker
in Depth).

Have a person stand at the origin and ensure their head is not cut off. Adjust
camera angles and such. Lock down the cameras, all of the adjustable screws and
bolts and such on your tripods. And lock the tripods to the floor with gaffer
tape.

### Finding your Cameras

The first step is to use `cwipc_register --noregister` to create a
`cameraconfig.json` file that simply contains the serial number of every
camera. If there already is such a file in the current directory this step does
nothing. Remove the `cameraconfig.json` file if you want to re-run.

Usually it will find what type of camera you have attached automatically. If
this fails you can supply the `--kinect` or `--realsense` option to help it.

### Coarse Registration

The easiest way to do coarse calibration is to put the origin marker on the
floor and run `cwipc_register --rgb --nofine`.

This will run a coarse calibration step for each camera in turn, but only if
the camera has not been coarse-calibrated before. In other words, you can run
this multiple times if some cameras were missed the previous time. But on the
other hand if you had to move cameras you should remove `cameraconfig.json` and
restart at the previous step.

For each camera, the RGB image is used to find the origin marker Aruco pattern.
The Depth image is then used to find the distance and orientation of the Aruco
marker from the camera. This information is then used to compute the 4*4
transformation matrix from camera coordinates to world coordinates, and this
information is stored in cameraconfig.json.

If the Aruco marker cannot be found automatically you can also use a manual
procedure, by not supplying the `--rgb` argument. You will then be provided with
a point cloud viewer window where you have to manually select the corners of
the marker (using shift-click with the mouse) in the right order.

After this step you have a complete registration. You can run `cwipc_view` to see
your point cloud. It should be approximately correct, but in the areas that are
seen by multiple cameras you will see the the alignment is not perfect.

### Fine Registration

If you have only a single RGBD camera there is no point in doing fine
calibration, but if you have multiple cameras it will slightly adjust the
registration of the cameras to try and get maximum overlap of the point clouds.

Have a person stand at the origin.

Run `cwipc_register`. If there is already a complete coarse calibration for all
cameras this will automatically do a fine calibration. If you are using
`cwipc_register --interactive` type a `w` to capture a point cloud and start the
registration.

The algorithm will iterate over the cameras, making slight adjustments to the
alignment. When it cannot improve the results any more it stops and saves
`cameraconfig.json`.

Check the results with `cwipc_view`.

The algorithm is not perfect, and it can some times get into a local minimum.
You will see this as a disjunct point cloud, often with cameras pairwise
aligned and those pairs disaligned.

The workaround is to try fine alignment again, with the subject standing in a
different pose.

At this point, if you view your point cloud with `cwipc_view` you will see that
it contains all the walls, floor, ceiling, furniture, etc.

Currently you have to fix this by manually editing `cameraconfig.json`. It
should be possible to edit `cameraconfig.json` while `cwipc_view` is running
and then typing `c` to reload `cameraconfig.json`. But this does not always
work, you may have to stop and restart cwipc_view to see the result of your
edits.

#### Near and Far Points

The first thing to edit is `threshold_near` and `threshold_far`. These are the
near and far point for all depth cameras (in meters). Adjust these to get rid
of most of the walls, while keeping the whole target area visible. Looking at
the floor is a good way to determine how you are doing.

#### Radius

Only for Kinect there is another parameter you can play with: `radius_filter`
applies a cylindrical filter around the origin.

#### Repeat Fine Calibration

You now have a clean capture of the subject without walls and furniture, but
with the floor still visible. This may be the best time to do the fine
calibration.

#### Remove Floor and Ceiling

Next, adjust `height_min` and `height_max` to get rid of floor and ceiling.
Both have to be non-zero otherwise the filter will not be applied (but
`height_min` can be less than zero if you want to keep the floor visible.

#### Color Matching

You probably want to play with the various exposure parameters such as
`color_whitebalance` and `color_exposure_time` to get the best color fidelity,
but you really have to experiment here.
