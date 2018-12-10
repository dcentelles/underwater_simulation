# UWSim-NET: UWSim with Network Simulator
This is a fork of https://github.com/uji-ros-pkg/underwater_simulation.
The 'comms' branch intregates a Network Simulator to be used along with the dccomms API. This simulator uses the NS3 libraries and the AquaSim NG as a NS3 module.

### Installation
We recommend the following steps in order to install the UWSim with the Network Simulator (you can jump to the step 3 if you have already installed the UWSim from source):
1. Create a catkin workspace and place a .rosinstall file inside it with the following contents:
```
- other: {local-name: /opt/ros/kinetic/share/ros}
- other: {local-name: /opt/ros/kinetic/share}
- other: {local-name: /opt/ros/kinetic/stacks}
- setup-file: {local-name: /opt/ros/kinetic/setup.sh}
- git: {local-name: src/underwater_simulation,
        uri: 'https://github.com/uji-ros-pkg/underwater_simulation.git', version: kinetic-devel}
``` 
2. Then, in your catkin workspace run the following command to download the sources:
```bash
$ rosws update
```
3. Go into the downloaded *underwater_simulation* in the src directory and add the *dcentelles/underwater_simulation* as a new "*git remote*":
```bash
$ git remote add dcent https://github.com/dcentelles/underwater_simulation.git
```
4. Fetch all branches from the remotes and checkout the *comms* branch:
```bash
$ git fetch --all
$ git checkout comms
```
5. Go again to src directory and clone the *dccomms_ros_pkgs* with the --recursive option (we recommand to first cache the github credentials):
```bash
$ git config --global credential.helper cache
$ git clone --recursive https://github.com/dcentelles/dccomms_ros_pkgs.git
```
6. Then go to the root of your catkin workspace and install the remaining ROS dependencies:
```bash
$ rosdep install --from-paths src --ignore-src --rosdistro kinetic -y
```
7. Compile the entire workspace with catkin_make:
```bash
$ catkin_make
```
8. Source the catkin_workspace:
```bash
$ source devel/setup.bash
```
9. Run the UWSim for the first time (if you have not already done so) and type Y to create the simulator directories:
```
$ rosrun uwsim uwsim
The UWSim data directory (~/.uwsim/data) does not exist. We need to download ~300Mb of data and place it under ~/.uwsim/data. Note this is required to run UWSim.
Continue (Y/n) ?
```
10. Close UWSim
### Install example network scenes
Go to the data/scenes subdirectory of the uwsim package:
```bash
$ roscd uwsim
$ cd data/scenes
```
Install the network scenes:
```bash
$ ./installScene -s netsim_scenes.uws
```
This will install the following scenes:
+ **netsim_custom_halfd.xml**: a simple scene with 2 BlueROVs. A custom communication device is attached to each BlueROV. In order to simulate a Half-Duplex link these modems are attached to the same communication channel for sending and receiving.

<p align="center"><img src="https://drive.google.com/uc?id=1x9h9tOj-i3hRySSriu30Cbj8lx5CYCYE" width="50%" /></p>

+ **netsim_twinbot.xml**: scene representing the scenario of the TWINBOT project. In this project, two AUV will launch a cooperative manipulation of an underwater pipe. The communication between them will be through a half-duplex wireless link based on the S100 RF modems of the WFS company. One of the UV communicates with a buoy by using the Evologics S2CR modems. The supervisor/operator is connected to the buoy through a Wi-Fi link (which is not modeled yet)

<p align="center"><img src="https://drive.google.com/uc?id=1JEbBEzJ7iytCHK_BLFmRyzvOikcwLq-Z" width="50%" /></p>

+  **netsim_shipwreck_bluerov_seabotix**: A shipwreck scenario with a surface boat and two ROVs (Seabotix and BlueROV2) using the S100 RF modems. The communication between the ROVs is stablished through a half-duplex wireless link.

<p align="center"><img src="https://drive.google.com/uc?id=1sQDEY5-3MGbCf0ysCtwnLT5rCcKGFQh7" width="50%" /></p>

In the same directory, run any network scene:
```bash
rosrun uwsim uwsim --configfile netsim_scene.xml
```
