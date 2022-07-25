# <p align="center">Collect and display a dataset in augmented reality scenario</p>

# About the project

**The project aims to the collection and visualization of a dataset in an augmented reality scenario**.<br/>It has been developed by Giovanni Ambrosi and Kevin Depedri for the Computer Vision course a.y. 2021-2022 Master in Artificial Intelligent Systems at the University of Trento. **The app is intended for Android devices only**.  



# Getting started
## Setup of the Unity Environment

* Install Unity editor 2021.3 or later versions;

* Install all the following required packages from `Window -> Package Manager`:
* -AR Foundation
* -ARCore XR Plugin
* -ARCore Extensions (link: ....)

* Connect to the ARCore Cloud Anchor API from  `Edit --> Project Settings -> XR Plug-in Management -> ARCore Extensions` by selecting the option `API Key` from the drop-down menu `Android Authentication Strategy` and pasting the following key `AIzaSyAfzBVfUoXcPlFHWhEW8Xl5K8NBVBe5RBI`

* If the previous `API Key` does not work please follow this procedure to generate your own API Key: (link: .....)



## Build of the application

* Download this repo and open the "Sample Scene" file or clone this repo from Unity;

* Click on `File -> Build Settings` and from the open window switch platform clicking on `Android` and then `Switch Platform`;

* Connect your mobile phone to the computer;

* Choose your device from the `Run Device` section;

* Click `Build and Run`;



# Description
## Structure of the script
The application is based in 2 main classes: ARPlacementManager and ARCloudAnchorManager, together with 3 minor classes ARDebugManager, SaveManager and AnchorEntity. 

### ARPlacementManager
The ARPlacementManager class handles all the procedure with regards to the screenshot object present in the environment, such as:
* Acquisition and placement of new screenshot objects (which initially are just a local objects and are not on the cloud yet). The new objects are instantiated with the current position and rotation of the acquisition device exploiting a cartesian coordinates system (x,y,z) and a quaternion
* Removal of one or more placed screenshot objects from the environment (useful to remove objects of which we are not satisfied before that these are hosted on the cloud)
* Recreation and placement of old screenshot objects (which are retrieved from the cloud after the hosting procedure)

### ARCloudAnchorManager
The ARCloudAnchorManager class handles all the procedure with regards to hosting and resolution of the screenshot object on the cloud, such as:
* First host, ...
* Successive host
* First resolution
* Successive resolution

### ARDebugManager, SaveManager and AnchrEntity
The ARDebugManager class allows us to get feedbacks from the application, while the SaveManager class together with the AnchrEntity class handles the save of the current session locally, allowing it to be retrieved in the next sessions or to be overwritten with a new one.


## Detailed working of the application
### Start of the application
As the application starts, the user is required to scan the environment for at least 15/30 seconds. This allow the SLAM algorithm to acquire some data from the current environment, and to build a feature map that will be used to position the anchors of the placed screenshoot objects. A longer scan allows to get a higher quality feature map, and so higher quality anchors which will be quicker to resolve and also more precise. A shorter scan leads to lower quality feature map which in the worst case could lead to anchors that are not retrievable.
It is important to remember that environment with extreme light conditions(brightness or darkness) and with important reflections will be very difficult to map, also when performing a scanning procedure for 30 seconds or more. In this case the placed object could be positioned in imprecise positions and the anchors could be really difficult to retrieve.

### Moving the first steps
After performing the scan of the environmente the user is able to place as many screenshot objects as he wants. To place an object it is sufficient to perform `One-Tap` on the area of the screen where the acquisition of the camera is visible. When an object is placed the user need to move slightly backward to be able to see the performed acquisition, which will be positioned with the correct inclination and also with the correct rotation (vertical or horizontal).

### Dealing with a multiple list system
The application is based on a multiple list system which allows to have different list for local objects and cloud object. In this way the user is able to acquire the wanted screenshot locally, then to evaluate them and eventually to remove the not good ones pressing the `Remove Last Obj button` to then acquire them again. Fianlly the users is able to host them when he is satisfied with the result pressing the `Host Anchors button`.
As soon as the `Host Anchors button` is pressed then the anchors are gradually hosted on the cloud and the removed from the list of the anchors which were waiting for the hosting, in this way each single object won't be hosted more than once.

### Different combinations of actions possibile
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



# Demo (GIFs or pictures)

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
Giovanni Ambrosi - (giovanni.ambrosi@studenti.unitn.it) -

Kevin Depedri - (kevin.depedri@studenti.unitn.it) -
