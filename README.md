# Diana Mission Execution with Testscript

## Introduction
This document provides step-by-step instructions for using the Diana Mission-Control Dashboard. The platform facilitates mission planning, device configuration, and execution on a decentralized network.

---

## Table of Contents
1. [Login](#login)
2. [Account Creation](#account-creation)
3. [Device Creation](#device-creation)
4. [Mission Creation](#mission-creation)
5. [Map Waypoints Data Creation and Execution](#map-waypoints-data-creation-and-execution)
6. [Conditions and Commands](#conditions-and-commands)
7. [Execute a Mission](#execute-a-mission)

---

## Login
1. Open the dashboard: [Diana Mission-Control](https://diana-dev.secublox.com)
2. Enter your **Username** and **Password**.
3. Click **Login**.
4. Upon successful login, you will be redirected to the dashboard.

**API Endpoint:**
```bash
POST https://api-diana-dev.secublox.com/api/auth/login
```

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

## Map Waypoints Data Creation and Execution
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

    Example Data:
    - Automatic: 1
    - Delay: 10
    - HAGL: 15
    - IED: 0
### Create a Command
1. Navigate to **Missions**.
2. Select a mission and click **Create Command**.
3. Enter command details:
   - Condition Name
   - Identifier
   - Mission
   - Device
   - Command
   - Action
   - Parameter
   - Order
4. Click **Save**.

    Example Data:

    - parameter: { "waypoints": [ { "order": 1, "latitude": 48.70948, "longitude": 11.53796, "hagl" :15, image_capture: true }, { "order": 2, "latitude": 48.7125, "longitude": 11.54899, "hagl" :15 }, { "order": 3, "latitude": 48.7144, "longitude": 11.55600, "hagl" :15 }, { "order": 4, "latitude": 48.717, "longitude": 11.56570, "hagl" :15 }, { "order": 5, "latitude": 48.7206, "longitude": 11.57686, "hagl" :15 }, { "order": 6, "latitude": 48.724, "longitude": 11.58692, "hagl" :15 }, { "order": 7, "latitude": 48.728, "longitude": 11.59999, "hagl" :15 }, { "order": 8, "latitude": 48.731, "longitude": 11.6121, "hagl" :15 }, { "order": 9, "latitude": 48.7336, "longitude": 11.629, "hagl" :15 }, { "order": 10, "latitude": 48.7334, "longitude": 11.65, "hagl" :15 }, { "order": 11, "latitude": 48.733, "longitude": 11.6618, "hagl" :15 }, { "order": 12, "latitude": 48.7317, "longitude": 11.6733, "hagl" :15 }, { "order": 13, "latitude": 48.724, "longitude": 11.6703, "hagl" :15 }, { "order": 14, "latitude": 48.718, "longitude": 11.663, "hagl" :15 }, { "order": 15, "latitude": 48.711, "longitude": 11.65, "hagl" :15 }, { "order": 16, "latitude": 48.705, "longitude": 11.635, "hagl" :15 }, { "order": 17, "latitude": 48.697, "longitude": 11.611, "hagl" :15 }, { "order": 18, "latitude": 48.691, "longitude": 11.593, "hagl" :15 }, { "order": 19, "latitude": 48.685, "longitude": 11.571, "hagl" :15 }, { "order": 20, "latitude": 48.682, "longitude": 11.559, "hagl" :15 }, { "order": 21, "latitude": 48.679, "longitude": 11.545, "hagl" :15 }, { "order": 22, "latitude": 48.675, "longitude": 11.53, "hagl" :15 }, { "order": 23, "latitude": 48.673, "longitude": 11.515, "hagl" :15 }, { "order": 24, "latitude": 48.672, "longitude": 11.5 }, { "order": 25, "latitude": 48.6699, "longitude": 11.48, "hagl" :15 }, { "order": 26, "latitude": 48.669, "longitude": 11.46, "hagl" :15 }, { "order": 27, "latitude": 48.668, "longitude": 11.44, "hagl" :15 }, { "order": 28, "latitude": 48.669, "longitude": 11.42, "hagl" :15 }, { "order": 29, "latitude": 48.6708, "longitude": 11.4114, "hagl" :15 }, { "order": 30, "latitude": 48.675, "longitude": 11.4145, "hagl" :15 }, { "order": 31, "latitude": 48.679, "longitude": 11.422, "hagl" :15 }, { "order": 32, "latitude": 48.684, "longitude": 11.4345, "hagl" :15 }, { "order": 33, "latitude": 48.689, "longitude": 11.4505, "hagl" :15 }, { "order": 34, "latitude": 48.6939, "longitude": 11.4705, "hagl" :15 }, { "order": 35, "latitude": 48.6981, "longitude": 11.4905, "hagl" :15 }, { "order": 36, "latitude": 48.7024, "longitude": 11.5095, "hagl" :15 }, { "order": 37, "latitude": 48.7046, "longitude": 11.5190, "hagl" :15 }, { "order": 38, "latitude": 48.70668, "longitude": 11.5270, "hagl" :15 }, { "order": 39, "latitude": 48.70930, "longitude": 11.53710, "hagl" :15 } ] }
---

## Execute a Mission
1. Go to **Missions**.
2. Select a mission.
3. Click **Execute Selected Missions**.
4. Confirm execution in the popup window.


---
