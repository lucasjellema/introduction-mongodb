# Quickstart environment with MongoDB

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/lucasjellema/introduction-mongodb)


Connect the MongoDB VS Code Extension to the local MongoDB database using this connection string: `mongodb://localhost:27017/`

Ctrl+Shift+P (or Cmd+Shift+P)
From the Command Palette, select MongoDB: Open Overview Page.

In the Overview page, click Connect under the Connect with Connection String.

Paste your connection string  `mongodb://localhost:27017/` into the Command Palette.

Press the Enter or Return key.

Check the docs: [MongoDB for VS Code - Connect to Your MongoDB Deployment](https://www.mongodb.com/docs/mongodb-vscode/connect/)

## Navigate the data

Check the docs: [Navigate Your Data](https://www.mongodb.com/docs/mongodb-vscode/databases-collections/)

# Gitpod Environment definition
In brief the definition of this Gitpod environment:

* MongoDB 5.0.2 on Ubuntu 20.4
* VS Code with MongoDB extension
* mongosh CLI (read more at [MongoDB Shell (mongosh)](https://www.mongodb.com/docs/mongodb-shell/))

Created through:
* based on custom Docker file (in .gitpod.yml: `image: file : .gitpod.Dockerfile`)
* custom Docker file .gitpod.Dockerfile based on image gitpod/workspace-mongodb ; check out this image at (Dockefile for Gitpod MongoDB image)[https://github.com/gitpod-io/workspace-images/tree/main/chunks/tool-mongodb] (this comes with MongoDB installed); custom steps in Docker file to download and install Mongo Shell CLI (mongosh)
* create data directory within /workspace on first startup (in .gitpod.yml: Task Run MongoDB `init: mkdir -p /workspace/data`) 
* run MongoDB service on every startup of the workspace (in .gitpod.yml: Task Run MongoDB `command: mongod --dbpath /workspace/data`)
* install MongoDB extension for VS Code (in .gitpod.yml: `vscode extensions :  mongodb.mongodb-vscode `)
* port 27017 is the port on which the MongoDB service is listening; this port is opened

* open document in editor in markdown preview: .vscode/settings.json: workbeanch editor associations and .gitpod.yml : gp open <document>