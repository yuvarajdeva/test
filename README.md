# Diana Mission Execution with Testscript

## Introduction
This document provides step-by-step instructions for using the Diana Mission-Control Dashboard. The platform facilitates mission planning, device configuration, and execution on a decentralized network.

---

## Table of Contents
1. [Login](#login)
2. [Account Creation](#account-creation)
3. [Device Creation](#device-creation)
4. [Mission Creation](#mission-creation)
5. [Map Waypoints Data Creation](#map-waypoints-data-creation)
6. [Conditions and Commands](#conditions-and-commands)
7. [Execute a Mission](#execute-a-mission)

---

## Login
1. Open the dashboard: [Diana Mission-Control](https://diana-dev.secublox.com)
2. Enter your **Username** and **Password**.
3. Click **Login**.
4. Upon successful login, you will be redirected to the dashboard.


---

## Account Creation
1. Navigate to **Accounts** in the main menu.
2. Click **Create New Account**.
3. Fill in the following details:
   - Name
   - Profile
   - Username
   - Password
   - Email
   - Nation
   - Domain
   - Logo
   - Document
   - Company
   - Address
   - Phone
4. Click **Submit** to create the account.

---

## Device Creation
1. Go to **Devices** in the main menu.
2. Click **Create New Device**.
3. Enter the device details:
   - Device Name
   - Identifier
   - Select Military
   - Device Type (Air, Land, Sea, Space, Cyber)
   - Blockchain
   - Logo
4. Click **Save**.
5. The device is now registered and ready for mission assignment.

---

## Mission Creation
1. Go to **Missions** in the main menu.
2. Click **Create New Mission**.
3. Enter mission details:
   - Mission Name
   - Identifier
   - Devices (select from registered devices)
   - Select Blockchain Network
4. Click **Save**.
5. The mission is now configured and ready for execution.

---

## Map Waypoints Data Creation
### Upload Waypoints Data
1. Navigate to **Map Settings**.
2. Click **Create** in Action for particular mission.
3. Select a JSON file containing waypoints.
4. Click **Upload**.

### Configure Waypoint Display Settings
1. Enable/Disable **Show Waypoints**.
2. Enable/Disable **Show Waypoints Label**.
3. Select **Map Icon Type**.
4. Choose **Map Icon Color**.
5. Click **Save**

## Conditions and Commands
### Create a Condition
1. Navigate to **Missions**.
2. Select a mission and click **Create Condition**.
3. Fill in the condition details:
   - Condition Name
   - Identifier
   - Mission
   - Devices
   - Automatic
   - Delay
   - HAGL
   - IED
   - Action
4. Click **Save**.

`
    Example Data:

    Automatic: 1
    Delay: 10
    HAGL: 15
    IED: 0


### Create a Command
1. Navigate to **Missions**.
2. Select a mission and click **Create Command**.
3. Enter command details:
   - Condition Name
   - Identifier
   - Mission
   - Device
   - Command
   - Receiver Device (Visible only if the command data contains "BC_WAYPOINTS", otherwise hidden)
   - Action
   - Parameter
   - Order
4. Click **Save**.

Note: An image is captured only when image_capture is set to true in BC_WAYPOINTS; otherwise, no image is captured.

    Example Data:

    parameter: { "waypoints": [ { "order": 1, "latitude": 48.70948, "longitude": 11.53796, "hagl" :15, image_capture: true }, { "order": 2, "latitude": 48.7125, "longitude": 11.54899, "hagl" :15 }, { "order": 3, "latitude": 48.7144, "longitude": 11.55600, "hagl" :15 }] }
---

## Execute a Mission
1. Go to **Missions**.
2. Select a mission.
3. Click **Execute Selected Missions**.
4. Confirm execution in the popup window.


---

## Run the Test script
### Installation and Setup Instructions


1. Clone the Repository
Run the following command to clone the repository:
```bash
git clone https://gitlab.com/secublox-platform/DIANA/diana-library.git
cd diana-library
```
2. Install Dependencies
Navigate to the cloned repository and install required dependencies:
```bash
pip3.12 install -r requirements.txt
```
3. Run the Script
Execute the following command to run the script:
```bash
python3.12 MissionControlScript/creator_device.py
```

## Result

In the Image section:
The captured image details will be displayed.
Clicking on an image will open a popup with two tabs:
- Image View (shows the captured image).
- Metadata Info (displays the metadata details).




