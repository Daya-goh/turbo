Alternative way to setup (for cyclic) using turborepro and npm workspaces

1 repo method:

- client - react
- server - express

## Suggested directory structure

├── apps
│ ├── client
│ └── server
└── packages

## create app folder

## create client in app

create client folder by npm create vite

## create server in app

- important to create the `dev` and `start` script

- enter server

- add this to package.json
  "scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "dev": "server.js",
  "start" : "node server.js"
  },

- add express backbone
  const express = require("express");
  const app = express();
  const port = process.env.PORT ?? 3000;

app.use(express.static("../client/dist"));

app.get("/", (req, res) => {
res.send("Hello World!");
});

app.listen(port, () => {
console.log(`Example app listening on port ${port}`);
});

## Turborepo

- go to root folder
  - create root folder's package.json
    npm init -y
    npm i turbo -D

turbo runs on concept workspaces

- add to package.json in app folder
  "workspaces": [
  "packages/*",
  "apps/*"
  ]

- touch turbo.json

- add to turbo.json
  {
  "$schema": "https://turborepo.org/schema.json",
  "pipeline": {
  "dev": {},
  "build": {
  "dependsOn": ["^build"],
  "outputs": ["dist/**"]
  },
  "test": {
  "dependsOn": ["build"],
  "outputs": [],
  "inputs": ["src/**/*.jsx", "src/**/*.js", "test/**/*.js", "test/**/*.jsx"]
  },
  "lint": {
  "outputs": []
  },
  "deploy": {
  "dependsOn": ["build", "test", "lint"],
  "outputs": []
  }
  }
  }

- touch .gitignore
- add content to .gitignore file
- git init
- git add -A
- git commit -m 'message'

- npx turbo dev
- npx turbo build

## cyclic

- git clone
- npm install
- npm run build
- npm run prune
- npm run start
