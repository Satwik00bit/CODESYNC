# CODESYNC

This application is a real-time collaborative code editor designed for seamless pair programming, technical interviews, and remote developer collaboration.

- In the application, you can create or join shared persistent rooms to write and edit code simultaneously with instantaneous updates.
- The platform leverages low-latency WebSockets to handle character-by-character code synchronization across all connected clients.
- The goal of the app is to maintain live user presence tracking with dynamic custom avatars and automatically preserve document states to prevent data loss.

>*The application is written using html, css, js, react, react router, node.js, express, socket.io, mongodb, mongoose.*

---

## 🚀 Key Features

* **Instant Real-Time Synchronization:** Low-latency, character-by-character code sync powered by WebSockets.
* **Persistent Room Architecture:** Code states are preserved instantly in a cloud database, preventing any data loss on disconnects.
* **Live Presence Tracking:** Real-time visibility of active users within each room using dynamic custom avatars.
* **Developer-Centric Interface:** High-contrast dark theme with responsive input control forms and built-in toast notifications.

---

## 🛠️ Tech Stack

| Layer | Technologies Used |
| :--- | :--- |
| **Frontend** | React, CodeMirror, UUID, React Hot Toast |
| **Backend** | Node.js, Express |
| **Real-Time Engine** | Socket.io, Socket.io-Client |
| **Database ODM** | MongoDB, Mongoose |

---

## 💻 Start-up instructions

*You can view the finished version of the site by clicking on the link:* (https://codesync-grwm.onrender.com)

#### If you want to run the site on your computer, follow these instructions:

1. Make sure you have an LTS version of Node.js installed on your computer. [Download and install](https://nodejs.org/) it if necessary.
2. Clone this repository.
3. Using the terminal or IDE, navigate to the project folder.
4. Create a `.env` file in the root directory to configure your environment variables:
   ```env
   MONGODB_URI=your_mongodb_atlas_connection_string
   PORT=5000
5. Install the project's base dependencies with the npm install command.
6. Start the application by executing the npm run build and then npm start commands.
7. Done! Go to the following address in your browser: http://localhost:5000.
