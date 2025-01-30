# Diana Mission-Control Dashboard

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

---

## Execute a Mission
1. Go to **Missions**.
2. Select a mission.
3. Click **Execute Selected Missions**.
4. Confirm execution in the popup window.


---
