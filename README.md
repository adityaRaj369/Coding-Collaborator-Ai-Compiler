
Deployed Link:    https://coding-collaborator-ai-compiler.vercel.app

![](https://raw.githubusercontent.com/PKief/vscode-material-icon-theme/ec559a9f6bfd399b82bb44393651661b08aaf7ba/icons/folder-markdown-open.svg)
#(Real-Time Coding Collaborator with Gemini AI Code Syggestions and on board Compiler With Code Save Feature)

![license](https://img.shields.io/github/license/adityaRaj369/CodeFuse?style=default&logo=opensourceinitiative&logoColor=white&color=0080ff)

![last-commit](https://img.shields.io/github/last-commit/adityaRaj369/CodeFuse?style=default&logo=git&logoColor=white&color=0080ff)

![repo-top-language](https://img.shields.io/github/languages/top/adityaRaj369/CodeFuse?style=default&color=0080ff)

![repo-language-count](https://img.shields.io/github/languages/count/adityaRaj369/CodeFuse?style=default&color=0080ff)
Video Demonstration:

https://github.com/user-attachments/assets/110a9a26-f3f9-4a18-9510-8b096f7e03ab

Table of Contents

Overview
Features
Project Structure
Directory Structure


Getting Started
Prerequisites
Installation
Usage
Deployment
Testing


Project Roadmap
Contributing
License
Acknowledgments


Overview
CodeFuse is a real-time, interactive coding platform designed for collaborative programming. It enables multiple developers to write, edit, and execute code simultaneously in a shared environment. With features like AI-powered code suggestions, Google authentication, and a Dracula-themed code editor, CodeFuse enhances productivity and teamwork for developers of all levels.
Deployed Link: https://codefuse-c88s.onrender.com

Features

Real-Time Collaboration: Multiple users can edit code simultaneously in a shared room, with changes synced instantly via Socket.IO.
AI-Powered Code Suggestions: Integrated with Google Gemini AI to provide real-time, context-aware code completions for languages like JavaScript, Python, Java, C++, and C.
Code Execution: Run code directly in the editor using Judge0 API, with support for input and output display.
Code Saving: Save code snippets to MongoDB, associated with user emails, with options to retrieve and delete saved codes.
Google Authentication: Secure login using Firebase Google Authentication for seamless user access.
Syntax Highlighting: Dracula-themed CodeMirror editor with support for JavaScript, Python, Java, C++, and C, including auto-close tags/brackets and line numbers.
Room Management: Generate unique room IDs for collaboration sessions and share them easily with a copy-to-clipboard feature.
Active Users Display: Shows all active users in a room with their usernames, emails, and profile photos.
Responsive UI: Modern, vampire-themed UI with modals for saving codes and viewing saved snippets.
Cross-Platform Support: Deployed on Vercel (frontend) and Render (backend) for reliable performance.


Project Structure
Directory Structure
CodeFuse/
├── client/                     # React frontend
│   ├── public/                 # Static assets
│   │   ├── images/             # Static assets (e.g., default-avatar.png, backgroundLogin.gif)
│   │   └── index.html          # HTML entry point
│   ├── src/                    # React source code
│   │   ├── components/         # Reusable components (Client.js, Editor.js)
│   │   ├── pages/              # Page components (Home.js, EditorPage.js)
│   │   ├── FireBase.js         # Firebase configuration for authentication
│   │   ├── Socket.js           # Socket.IO client configuration
│   │   ├── Actions.js          # Socket.IO event constants
│   │   ├── Editor.css          # Custom styles for CodeMirror editor
│   │   └── index.js            # React entry point
│   ├── .env                    # Frontend environment variables
│   └── package.json            # Frontend dependencies and scripts
├── server/                     # Node.js backend
│   ├── index.js                # Main server file (Express + Socket.IO)
│   ├── Actions.js              # Socket.IO event constants
│   ├── .env                    # Backend environment variables
│   └── package.json            # Backend dependencies and scripts
├── .gitignore                  # Git ignore file
└── README.md                   # Project documentation


Getting Started
Prerequisites

Node.js (v14 or higher)
npm (v6 or higher) or yarn
MongoDB (MongoDB Atlas or local instance)
Firebase Account for Google authentication
Google Gemini API Key for AI code suggestions
Judge0 API Key for code execution (via RapidAPI)

Installation

Clone the Repository:
git clone https://github.com/adityaRaj369/CodeFuse.git
cd CodeFuse


Install Frontend Dependencies:
cd client
npm install


Install Backend Dependencies:
cd ../server
npm install


Set Up Environment Variables:

Create a .env file in the client directory:REACT_APP_GEMINI_API_KEY=your_gemini_api_key
REACT_APP_JUDGE0_HOST=https://judge0-ce.p.rapidapi.com
REACT_APP_JUDGE0_API_KEY=your_judge0_api_key
REACT_APP_BACKEND_URL=http://localhost:5000


Create a .env file in the server directory:PORT=5000
MONGO_URI=your_mongodb_atlas_uri




Configure Firebase:

Update client/src/FireBase.js with your Firebase project credentials:const firebaseConfig = {
  apiKey: "your_api_key",
  authDomain: "your_auth_domain",
  projectId: "your_project_id",
  storageBucket: "your_storage_bucket",
  messagingSenderId: "your_messaging_sender_id",
  appId: "your_app_id",
  measurementId: "your_measurement_id"
};





Usage

Start the Backend Server:
cd server
npm start

The server will run on http://localhost:5000.

Start the Frontend Application:
cd client
npm start

The frontend will run on http://localhost:3000.

Access the Application:

Open http://localhost:3000 in your browser.
Log in with Google, create or join a room, and start coding collaboratively.



Deployment
Deploy Frontend on Vercel

Push the client directory to a Git repository (e.g., GitHub).
Log in to Vercel, create a new project, and import your repository.
Set the following environment variables in Vercel:
REACT_APP_GEMINI_API_KEY
REACT_APP_JUDGE0_HOST
REACT_APP_JUDGE0_API_KEY
REACT_APP_BACKEND_URL (e.g., https://your-backend.onrender.com)


Configure the project:
Framework Preset: Create React App
Root Directory: client
Build Command: npm run build
Output Directory: build


Deploy and access the provided Vercel URL.

Deploy Backend on Render

Push the server directory to a Git repository.
Log in to Render, create a new Web Service, and connect your repository.
Configure the service:
Root Directory: server
Runtime: Node
Build Command: npm install
Start Command: npm start
Environment Variables:
PORT=5000
MONGO_URI=your_mongodb_atlas_uri




Update CORS in server/index.js to allow the Vercel frontend URL.
Deploy and note the Render URL (e.g., https://your-backend.onrender.com).
Update REACT_APP_BACKEND_URL in Vercel and redeploy the frontend.

Deployed Link

Frontend and Backend: https://codefuse-c88s.onrender.com

Testing
Run tests for both client and server (if tests are implemented):
cd client
npm test

cd ../server
npm test

To test manually:

Log in with Google and verify authentication.
Create/join a room and test real-time code syncing.
Save and retrieve code snippets to confirm MongoDB integration.
Run code with inputs to verify Judge0 integration.
Type code to check Gemini AI suggestions.


Project Roadmap

Phase 1 (Completed): Implement real-time collaboration, AI code suggestions, Google authentication, code execution, and MongoDB storage.
Phase 2: Add user profile management and role-based access (e.g., room admin).
Phase 3: Integrate GitHub for code versioning and project import/export.
Phase 4: Enhance AI suggestions with more advanced models and add support for additional languages.
Phase 5: Implement in-app chat for real-time communication.
Phase 6: Optimize performance for large-scale collaboration and deploy on a scalable cloud platform (e.g., AWS).


Contributing
We welcome contributions! Follow these steps:

Fork the repository.
Create a new branch (git checkout -b feature/your-feature).
Commit your changes (git commit -m "Add your feature").
Push to the branch (git push origin feature/your-feature).
Open a pull request.

Please ensure your code follows the existing style and includes tests where applicable.

License
This project is licensed under the MIT License. See the LICENSE file for details.

Acknowledgments

Socket.IO: For enabling real-time collaboration.
CodeMirror: For the syntax-highlighted editor with Dracula theme.
Google Gemini AI: For intelligent code suggestions.
Judge0: For code execution capabilities.
Firebase: For secure Google authentication.
MongoDB: For persistent code storage.
Vercel and Render: For seamless deployment.
Inspired by collaborative tools like Visual Studio Live Share and CodePen.

