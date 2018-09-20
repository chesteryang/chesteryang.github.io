---
layout: post
title:  "Deploy nodejs/graphql project to Heroku"
date:   2018-09-20 11:00:00 -0600
categories: nodejs
---

Deploy graphql yoga/typescript/typeorm/chinook project to Heroku
===================================================================================

The project's github repo is [here](https://github.com/chesteryang/graphql-ts){:target="_blank"}.

1. The first task to take care of is environment variable: process.env.PORT. Heroku will set this variable to a port number for the project's expressjs server to listen to. So in the project we have such statement (src/startServer.ts):
```JavaScript
    const port = process.env.PORT || 8030;
```
If there is a value on process.env.PORT, the server will listen to it as Heroku will direct all requests to this port.

2. You can create a free account with nodejs type so push code to Heroku cannot be simpler as git push. 

3. But the project just doesn't work after the push as the project is in typescript setup. We need a way to compile to Javascript. Heroku provides a hook to do it and you can take a look at project.json:
```json
      "heroku-postbuild": "npm run build"
```
"heroku-postbuild" provides a chance to run script after npm install and before pruning dev dependencies. We utilize this chance to do compile and package the application into dist folder. Heroku will look into "Profile" file to start the application:
```
web: node dist/index.js
```

4. The project uses typeorm as sqlite database orm and ormconfig.json will need the setup for *.js entities etc.

The deployed website is [here](https://chinook-gql.herokuapp.com/){:target="_blank"}. As the webiste is on dynos, it will take a while to load for the first time.
