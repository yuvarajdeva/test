<div align="center">

  <p align="center">
    <h2> 🚀 Diana Frontend </h2>
  </p>
  This repository contains the frontend implementation of Diana, a decentralized C2 Mission Management System. Built using React + Vite, it provides a seamless interface for managing missions, devices, and blockchain interactions.

</div>

## 🛠️ Getting Started
**Prerequisites**
Ensure you have the following installed before proceeding:

- Node.js (v18.0.0 or higher)
- npm or yarn (package manager)
**Installation**

1. Clone the repository and install dependencies:

```bash
  $ git clone https://gitlab.com/secublox-platform/DIANA/frontend.git
  $ cd frontend
  $ npm install or yarn install  
```
2. Run the local development server:
```bash
  $ npm run dev or yarn dev  
```
Your application will be accessible at <http://localhost:5173>

## 📂 Project Structure
``` bash
/frontend  
│── README.md             # Documentation  
│── package.json          # Project dependencies  
│── vite.config.js        # Vite configuration  
│── public/               # Static assets  
│── src/                  # Main source code  
│   ├── assets/           # Fonts, images, and styles  
│   ├── components/       # Reusable UI components  
│   ├── pages/            # Application pages  
│   ├── store/            # Redux state management  
│   ├── helpers/          # Utility functions  
│   ├── routes/           # Route definitions  
│   ├── constants/        # Static configuration values  
│   ├── serviceWorker.js  # Service worker setup  
│   ├── index.js          # Entry point  
│   └── App.jsx           # Main application component  
```

## 🚀 Features
✅ **Role-Based Access Control**  
   - Administrator, Operator, Military  

✅ **Account Management**  
   - Create/Delete Military or Operator account based on the Role-Based Access Control

✅ **Mission Management**  
   - Create(Mission, Command, Condition and waypoints), Execute, and Track(Request)

✅ **Device Management**  
   - Swarm devices, Configuration, and Status updates
     
✅ **Image**  
   - Captured Image Metadata Info Track

✅ **Blockchain Integration**  
   - Smart Contracts, Transactions, and execution  

✅ **Map & Waypoints**  
   - Geolocation-based mission planning  

✅ **Secure Authentication**  
   - JWT-based login  

## Customization

You can customize the Vite configuration by editing the vite.config.js file. Refer to the [Vite documentation](https://vite.dev/config/) for more details on available configuration options.

## Acknowledgements

- [React](https://react.dev/)
- [Vite](https://vite.dev/)
