# Monorepo

Alternative way to setup (for cyclic) using turborepo & npm workspaces

* client - react
* server - express

## Suggested directory structure

├── apps
│   ├── client
│   └── server
└── packages

## Client

Standard vite project

## Server

Important create `dev`

```js title="package.json"
{
  "name": "server",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "dev": "nodemon server.js",
    "start": "node server.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.20"
  }
}
```

```js title="server.js"
const express = require("express");
const app = express();
const port = process.env.PORT ?? 3000;

//* ?? to fill in later
app.use(express.static("??"));

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
});
```

## Turborepo

Go to the root folder, create root package.json

```js title="turbo.json"
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
```

## Workspaces

npm, yarn, pnpm

```js title="package.json"
  "workspaces": [
    "packages/*",
    "apps/*"
  ]
```

Also add these scripts:

```js
  "scripts": {
    "build": "turbo build",
    "start": "cd apps/server && npm run start"
  },
```

## Cyclic

`git clone`
`npm install` - workspace
`npm run build`
`npm run prune` - drops dev dep
`npm run start`
