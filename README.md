# Diana - Decentralized C2 on Blockchain

## Introduction

This document outlines the key requirements and provides a step-by-step tutorial for the technology demonstrator of Decentralized C2 on Blockchain. It serves as a guide for utilizing the Mission-Control Dashboard, simulating configured devices, and executing predefined missions efficiently.

## Technical Description

### Overview

- A **web-based dashboard**, built using **Django** and **React**, provides **role-based access** for stakeholders such as Administrators, Operators, and Military personnel. 
- Integration with a **decentralized network (public blockchain)** using **Solidity smart contracts** ensures transparent mission control and execution.

## Mission-Control Dashboard

The dashboard, along with a backend (database, REST API), facilitates mission management for various stakeholders to configure and execute mission scenarios.

### Key Features
- **Log in**
- **Accounts Management**
- **Devices Management**
- **Images Handling**
- **Missions Execution**
- **Wallets Integration**

### Login Details
- **Dashboard URL:** [Diana Dev](https://diana-dev.secublox.com)
- **API Authentication:** [`https://api-diana-dev.secublox.com/api/auth/login`](https://api-diana-dev.secublox.com/api/auth/login)

## User Roles

### 1. **Administrator**
   - Oversees the platform with full read and write permissions.
   - Manages deployment and system operations.

### 2. **Operator**
   - Executes missions and allocates assets (Military, Wallets).

### 3. **Military**
   - Authorizes assigned devices for approved missions.
   
### 4. **Device**
   - Represents an autonomous unit (Air, Land, Sea, Space, Cyber).
   - Status monitoring and configuration.

## Account Settings

- View and edit profile information.
- Change login password.
- Manage user accounts and roles.

### Features:
- **Create new accounts**
- **Assign wallets to user profiles**
- **Edit/Delete accounts**
- **Search for accounts**

### Data Fields:
- Name, Profile, Username, Email, Nation, Domain, Logo, Document, Company, Address, Description, Phone

## Devices Management

Devices form the core of the autonomous swarm system, representing operational units.

### Access Control:
- **Administrator:** Full access
- **Operator:** Limited to their own devices
- **Military:** Can authorize devices for missions

### Features:
- **Register & Manage Devices**
- **Assign to missions & Military units**
- **Edit/Delete/Search devices**

### Data Fields:
- Device Name, Identifier, Military Assignment, Type, Blockchain, Logo

## Images Handling

Users can capture and securely store images. 

### Image Display Features:
- **Image View Tab:** Displays captured images
- **Metadata Tab:** Shows structured metadata
- **Automatic Update:** Live information updates every 5 seconds

## Missions Execution

Missions integrate all elements of the decentralized C2 system.

### Access Control:
- **Administrator:** Full access to all missions
- **Operator:** Limited to their missions

### Features:
- **Create new missions**
- **Configure mission parameters**
- **Assign devices to missions**
- **Monitor mission status**
- **Edit/Delete missions**

### Data Fields:
- Mission Name, Identifier, Devices, Blockchain Network

## Mission Execution

### Steps:
1. **Select a mission**
2. **Click 'Execute Selected Missions'**
3. **Confirm execution in popup**

### Mission Commands:
- **Define flight paths, waypoints, takeoff, arm/disarm operations**
- **Executed via smart contract transactions**

## Conditions & Commands

### Conditions:
- Manage mission-specific delays, HAGL, and IED alerts.
- **Data Fields:** Condition Name, Identifier, Mission, Devices, Delay, HAGL, IED

### Commands:
- Control various aspects of the mission.
- **Data Fields:** Condition Name, Identifier, Mission, Device, Command, Action, Parameters, Order

## Mission Requests & Tracking

- Tracks command transaction details, execution time, contract address status.

## Map Settings

- Displays mission waypoints with labels.
- Allows JSON-based modifications.

### Features:
- **Upload waypoints**
- **Enable/Disable waypoint display**
- **Customize map icons (type, color)**

---
