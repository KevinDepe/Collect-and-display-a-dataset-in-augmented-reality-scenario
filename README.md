# <p align="center">Collect and display a dataset in augmented reality scenario</p>

# About the project

**The project aims to the collection and visualization of a dataset in an augmented reality scenario**.<br/>It has been developed by Giovanni Ambrosi and Kevin Depedri for the Computer Vision course a.y. 2021-2022 Master in Artificial Intelligent Systems at the University of Trento. **The app is intended for Android devices only**.  

# Table of contents

* [SLAM Algorithm](#Slam-algorithm)

* [Getting started](#Getting-started)

* [Description](#Description)

* [Documentation and useful links](#Documentation-and-useful-links)

* [Demo](#Demo)

* [Future implementations](#Future-implementations)

* [Contacts](#Contacts)

# SLAM Algorithm

## Introduction

SLAM (**Simultaneous Localization and Mapping**) is an algorithm born in the 80s.
Its goal is to obtain a global and consistent estimate of a device’s path while reconstructing a map of the surrounding environment. 
The coupling between these two tasks, initially considered as the core issue, was soon discovered to be the real strength of SLAM methods.<br/> 
This duality has also encouraged its diversification. By dosing the importance given to mapping or to localization, SLAM has been pushed away from the sole robotics field and became a reference to solve problems of many different natures: from **micro aerial vehicles** to **augmented reality (AR) on a smartphone**.
**SLAM uses devices/sensors to collects visible data (camera) and/or non-visible data (RADAR, SONAR, LiDAR) with basic positional data collected using Inertial Measurement Unit (IMU)**.<br/>
Together these sensors collect data and build a picture of the surrounding environment and along woth the SLAM algorithm estimate the location and position within the surrounding environment.
Moreover SLAM algorithms use two main designs. The first design corresponds to filter-based solutions (Extended Kalman filter or particle filters), the second design utilizes parallel methods derived from PTAM (Parallel Tracking and Mapping). 
In this project we have focused mainly on visual-inertial SLAM (viSLAM) an evolution of visual SLAM (vSLAM) tecniques.

Mathematically speaking given:<br/>
<br/>![equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/15cd7daffb06c3fa7898e10fe16953895cb3a369) -> map of the environment;<br/>
<br/>![equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/f279a30bc8eabc788f3fe81c9cfb674e72e858db) -> agent's current state;<br/>
<br/>![equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/3cd2cf4bfdabc8ae396ce3fa32aeb871efb3d732) -> sensor's observation;<br/>
<br/>![equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/ffc45a5286dc4ff8b99c89d5fbf0c0b9760babf1) -> series of controls<br/>

the objective is to compute:<br/>

![equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/c607fe964e04d2d7c46f8420596205eb67737000)<br/>
<br/>Using Bayes's rule and given a map and a transition function ![equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/18bc6b8abe9221154012d7bf365049d4755902e8) we can compute:<br/>
<br/>![equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/a9af46b0bc5e00ee32f838783ee48004379e32a0)<br/>
Similarly the map can be updated sequentially by:<br/>
<br/>![equation](https://wikimedia.org/api/rest_v1/media/math/render/svg/15a2717a2788d8cb12aaa07295d6278ddbf7044b)<br/>

In the following we will introduce the main principles of vSLAM and viSLAM.<br/>

## Classical Structure of vSLAM
Four main blocks describe the overall operation of any vSLAM algorithm. They are the following:
* ***Input search***: finding the required information in the sensor measurements
* ***Pose tracking***: determining the current camera pose from the new perceptions
* ***Mapping***: adding a landmark to the map
* ***Loop closing***: producing a proper map and drift-free localization<br/><br/><br/>

![image_1](https://static-02.hindawi.com/articles/js/volume-2021/2054828/figures/2054828.fig.002.svgz)<br/><br/><br/>

## From vSLAM to viSLAM
As said in the prevoius rows viSLAM (and vSLAM too) methods use IMU in order to establish the current position and camera pose.
Direct and indirect features could be used to classify viSLAM methods. Some reviews have also classified viSLAM methods depending on whether they are filter or optimization-based methods. But most major viSLAM methods are actually feature-based methods and viSLAM mainly deals with hybridization issues. Therefore, the classification shown in Figure below is based on the coupling level of the visual and inertial data. We differentiate two levels: loose and tight coupling.<br/><br/><br/>

![image_2](https://static-02.hindawi.com/articles/js/volume-2021/2054828/figures/2054828.fig.003.svgz)<br/><br/><br/>

### Loose coupling

Loosely coupled methods process the IMU and image measurements separately and use both information to track the pose. IMU measurements can also be filtered to estimate rotations that are fused in an image-based estimation algorithm. Loosely coupled visual-inertial odometry method is one part of the global multisensor fusion (magnetometers, pressure altimeters, GPS receiver, laser scanners, …). Although the interest for visual-inertial systems is quite recent, works on loosely coupled IMU-camera fusion started already in the early 2000s. SOFT-SLAM algorithm is a loosely coupled viSLAM method that in fact uses IMU data to reduce computation time when available. It builds in real time a dense map and runs on a MAV.<br/><br/>
### Tight coupling

Instead of fusing the outputs of vision and inertial-based algorithms, tightly coupled methods fuse directly visual and inertial raw data to improve accuracy and robustness. The MSCKF and MSCKF 2.0, both robust and very light, belong to this category, along with ROVIO, which is an EKF-based direct VIO method. Kimera [60] is also based on a VIO method but it also includes a pose graph optimizer, in different threads, for global trajectory estimation, a 3D mesh reconstruction module, and a 3D metric-semantic reconstruction module. Its front-end extracts feature with ORB while its back-end runs graph optimization. But its main interest lies in a new IMU initialization method that first estimates the gyroscope’s bias, approximates the scale and the gravity (without considering accelerometer bias), and then estimates the accelerometer bias (with scale and gravity direction refinement) and finally the velocity vector. It includes global optimization and loop closure in parallel methods. Most of the recent viSLAM methods are tightly coupled, as the one presented by that uses forward and backward optical flow to tack image features.
# Getting started
## Setup of the Unity Environment

1) Install Unity editor 2021.3 or later versions;

2) Install all the following required packages from `Window -> Package Manager`:
   - **AR Foundation**;
   - **ARCore XR Plugin**;
   - **ARCore Extensions (link: ....)**;

3) Connect to the ARCore Cloud Anchor API from  `Edit -> Project Settings -> XR Plug-in Management -> ARCore Extensions` by selecting the option `API Key` from the drop-down menu `Android Authentication Strategy` and pasting the following key `AIzaSyAfzBVfUoXcPlFHWhEW8Xl5K8NBVBe5RBI`;

4) If the previous `API Key` does not work please follow this procedure to generate your own API Key: (link: .....);



## Build of the application

* Download this repo and open the "Sample Scene" file;

* Click on `File -> Build Settings` and from the open window switch platform clicking on `Android` and then `Switch Platform`;

* Connect your mobile phone to the computer;

* Choose your device from the `Run Device` section;

* Click `Build and Run`;



# Description




## Structure of the script
The application is based in 2 main classes: ARPlacementManager and ARCloudAnchorManager, together with 3 minor classes ARDebugManager, SaveManager and AnchorEntity. 

### ARPlacementManager
The ARPlacementManager class handles all the procedure with regards to the screenshot object present in the environment, such as:
* Acquisition and placement of new screenshot objects (which initially are just a local objects and are not on the cloud yet). The new objects are instantiated with the current position and rotation of the acquisition device exploiting a cartesian coordinates system (x,y,z) and a quaternion;
* Removal of one or more placed screenshot objects from the environment (useful to remove objects of which we are not satisfied before that these are hosted on the cloud);
* Recreation and placement of old screenshot objects (which are retrieved from the cloud after the hosting procedure);

### ARCloudAnchorManager
The ARCloudAnchorManager class handles all the procedure with regards to hosting and resolution of the screenshot object on the cloud, such as:
* First hosting request, it happens when the hosting procedure is performed for the first time in a session (without retrieving the previous session), it effectively creates a new session overwriting the previous one;
* Successive host, it happens all the times that a hosting procedure is performed after a previous hosting procedure, or after retrieving the prevous session. It allows to add elements to the current session on the cloud;
* First resolution, it appens when the resolution procedure is performed for the first time in a session (without hosting any element), it effectively retrieves the previous session, allowing to visualize it and to update it adding new screenshot objects;
* Successive resolution, it happens all the times that a resolution procedure is performed after a previous hosting procedure. It allows to retrieve all the screenshot objects that are currently missing in the scene. The objects are retrieved from the cloud;

### ARDebugManager, SaveManager and AnchrEntity
The ARDebugManager class allows us to get feedbacks from the application, while the SaveManager class together with the AnchrEntity class handles the save of the current session locally in a .Json file, allowing it to be retrieved in the next sessions or to be overwritten with a new one. [ADD WHAT REQUIRED ABOUT .JSON STORING]


## Detailed working of the application
### Start of the application
As the application starts, the user is required to scan the environment for at least 15/30 seconds. This allow the SLAM algorithm to acquire some data from the current environment, and to build a feature map that will be used to position the anchors of the placed screenshoot objects. A longer scan allows to get a higher quality feature map, and so higher quality anchors which will be quicker to resolve and also more precise. A shorter scan leads to lower quality feature map which in the worst case could lead to anchors that are not retrievable.
It is important to remember that environment with extreme light conditions(brightness or darkness) and with important reflections will be very difficult to map, also when performing a scanning procedure for 30 seconds or more. In this case the placed object could be positioned in imprecise positions and the anchors could be really difficult to retrieve.

### Moving the first steps
After performing the scan of the environmente the user is able to place as many screenshot objects as he wants. To place an object it is sufficient to perform `One-Tap` on the area of the screen where the acquisition of the camera is visible. When an object is placed the user need to move slightly backward to be able to see the performed acquisition, which will be positioned with the correct inclination and also with the correct rotation (vertical or horizontal).

### Dealing with a multiple list system
The application is based on a multiple list system which allows to have different list for local objects and cloud object. In this way the user is able to acquire the wanted screenshot locally, then to evaluate them and eventually to remove the not good ones pressing the `Remove Last Obj button` to then acquire them again. Fianlly the users is able to host them when he is satisfied with the result pressing the `Host Anchors button`.
As soon as the `Host Anchors button` is pressed then the anchors are gradually hosted on the cloud and the removed from the list of the anchors which were waiting for the hosting, in this way each single object won't be hosted more than once.

### Different possibile combinations of actions
This system of multiple list allows to handle any possible combinations of the `Host/Remove/Resolve procedures`, meaning that the user will be able to: start a session, take some screenshot, remove the bad ones and host the good ones, then add other elements to the previously hosted ones and so on. There is no cambination of these actions that is not supported by the script.

### Start a new session or retrieve a previous one
After that the acquisition has been performed, the anchors have been hosted and the application has been closed, when we are starting it again we can perform two different choices:
* If we want to `Start a fresh new session` then we need to start the application, acquire one or more screenshot, and only when we press the `Host Anchors button` then we will effectively overwrite the previous session with the current one.
* If we want to `Retrieve the previous session` then we need to start the application and press the `Resolve Anchors button`, in this way the previously acquired elements will be restored and from now moving on it will be possible to add new elements to that pevious session.


## Summarized phase of the application
<br/>The usage of the application is summarize in the following bullet list:

1) The first phase consists in scanning around the environment for at least 15/30 seconds;

2) After a tap on the screen the application takes a pic of the environment and creates an hologram with a local anchor associated;

3) The hologram is visible through the device camera as a plane with the picture as texture. It is placed in the ARcamera's coordinates with the same inclination and orientation;

4) After having placed the holograms the user has three choices:
   - **Host anchor**: upload the anchor points on the cloud;
   - **Remove last object**: delete the last hologram created;
   - **Resolve anchor**: retrieve the anchors uploaded previously on the cloud and place the holograms in the correct positions.


5) After closing and re-starting the application:
   - **Start a fresh new session**: place a screenshot object and host it to start a new session and overwrite the previous one;
   - **Retrieve the previous session**: resolve the previously hosted anchors to retrieve the previous session and update it.

<br/>**NOTE**: all the pictures are saved in a predefined path in the device (android/data/com.WreckerCompany.appname/files/SavedImage/Image-x.png)



# Documentation and useful links
* [Unity Documentation](https://docs.unity3d.com/Manual/index.html)

* [Unity Fundamentals](https://www.andreasjakl.com/ar-foundation-fundamentals-with-unity-part-1/)

* [Anchors and Raycasting](https://www.andreasjakl.com/ar-foundation-fundamentals-with-unity-part-3/)

* [SLAM Algorithm](https://gisresources.com/what-is-slam-algorithm-and-why-slam-matters/)

* [vSLAM and viSLAM methods)](https://www.hindawi.com/journals/js/2021/2054828/)



# Demo

![this is an image](https://github.com/GiovanniAmbrosi/Collect-and-display-a-datasets-in-augmented-reality-scenario/blob/main/10addio.gif?raw=true)



# Future implementations

- [ ] Add support for API to host also images and Json file on the cloud allowing for a full information retrieval from other devices;

- [ ] Add real-time information about the quaity of the generated feature map to encurage users to explore more the environment;

- [ ] Add the option to save the pictures in a path choosen by the user and to show the pictures in the image gallery;

- [ ] Add the possibility to remove a precise object aiming at it instead of removing the last one placed;

- [ ] Renderization of the back of the hologram to visualize holograms from every view angle;

- [ ] Extension of the anchors survival time up to 365 days;

- [ ] Training of neural networks (NeRF);



# Contacts
Giovanni Ambrosi - (giovanni.ambrosi@studenti.unitn.it)

Kevin Depedri - (kevin.depedri@studenti.unitn.it)
