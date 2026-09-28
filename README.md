# CODESYNC

A real-time collaborative code editor for pair programming, technical interviews, and remote collaboration. Open the same room in two or more browser tabs and every keystroke appears for everyone, with a live list of who is connected. Each room's code is saved in MongoDB, so it survives refreshes and disconnects.

**Live demo:** https://codesync-grwm.onrender.com

---

## Features

- **Live code sync**: every change is broadcast over Socket.io to everyone else in the room.
- **Persistent rooms**: the current code of each room is stored in MongoDB and loaded when someone joins.
- **Presence**: a live sidebar of connected users with generated avatars, plus join and leave notifications.
- **Share by Room ID**: create a new room ID (UUID), copy it, and send it to a collaborator.
- **Editor**: CodeMirror with line numbers, JavaScript syntax highlighting, and auto-closing brackets and tags, in a dark theme.

## How it works

1. On the home page you enter a Room ID (or generate one) and a username.
2. The editor page opens a Socket.io connection and emits `join`.
3. The server adds the socket to a Socket.io room, finds or creates the room's document in MongoDB, tells everyone in the room who is present, and sends the saved code to the new user.
4. When you type, the client emits `code-change`. The server broadcasts it to everyone else in the room and saves it to MongoDB.
5. When someone disconnects, the server notifies the room and the sidebar updates.

The whole app runs as **one Node.js process**: Express serves the built React app and hosts the Socket.io server on the same port, so no CORS setup is needed in production.

## Tech stack

| Layer | Technologies |
|---|---|
| Frontend | React 18, React Router 6, CodeMirror 5, react-hot-toast, react-avatar, uuid |
| Backend | Node.js, Express |
| Real time | Socket.io and socket.io-client (WebSocket transport) |
| Database | MongoDB with Mongoose |
| Build tooling | Create React App (`react-scripts`) |
| Hosting | Render |

## Project structure

```text
server.js              Express + Socket.io server, Mongoose connection, all event handlers
models/Room.js         Mongoose schema: roomId, content, lastModified
src/
  index.js, App.js     React entry and routes ( / and /editor/:roomId )
  Actions.js           Socket event names shared by client and server
  socket.js            Creates the socket.io-client connection
  containers/
    Home.js            Room ID and username form
    Editor.js          Socket connection, presence list, leave and copy actions
  components/
    MainContent.js     CodeMirror editor wired to the socket
    Client.js          One avatar and username row in the sidebar
public/                Static assets and index.html
```

## Getting started

Requirements: an LTS version of [Node.js](https://nodejs.org/) and a MongoDB database (a free [MongoDB Atlas](https://www.mongodb.com/atlas) cluster works, or a local MongoDB).

1. Clone the repository and install dependencies:

   ```bash
   git clone https://github.com/Satwik00bit/CODESYNC.git
   cd CODESYNC
   npm install
   ```

2. Create a `.env` file in the project root:

   ```env
   MONGODB_URI=your_mongodb_connection_string
   PORT=5000
   ```

   If `MONGODB_URI` is not set, the server falls back to `mongodb://localhost/cocode`. If `PORT` is not set, it uses `5000`.

3. Build the frontend and start the server:

   ```bash
   npm start
   ```

   This runs `npm run build` and then `node server.js`.

4. Open http://localhost:5000. To try collaboration, open the same room in a second tab or browser window with a different username.

### Scripts

| Command | What it does |
|---|---|
| `npm start` | Build the React app, then start the production server |
| `npm run build` | Build the React app into `build/` |
| `npm run server:prod` | Start the server (expects `build/` to exist) |
| `npm run server:dev` | Start the server with nodemon (restarts on server file changes) |
| `npm run start:front` | Start the React dev server on port 3000 (see the note below) |

> **Note on running the frontend and backend separately:** the React dev server (port 3000) and the backend (port 5000) are different origins, and the Socket.io server currently has no CORS configuration. Running them separately therefore needs extra setup (a `REACT_APP_BACKEND_URL` variable pointing at the backend, plus a `cors` option on the Socket.io server). The steps above avoid this by serving everything from the one server.

## Deployment

Deployed on Render as a single web service. Set `MONGODB_URI` in the service's environment variables. Use `npm install && npm run build` as the build command and `node server.js` as the start command.

## Known limitations

- **No authentication**: anyone with a Room ID can join and edit that room. A username is only a display name.
- **Last write wins**: each change replaces the whole document, so several people typing at the exact same moment can overwrite each other. There is no merge logic (CRDT or operational transform).
- **One server instance**: presence is tracked in memory, so running multiple instances would need a shared adapter such as Redis.
- **Rooms never expire**: saved rooms stay in MongoDB until deleted manually.
- **Layout**: the home page is responsive, the editor layout is not tuned for small screens yet.
- **Language support**: only JavaScript syntax highlighting is loaded.
- **Tests**: no working automated tests yet.
