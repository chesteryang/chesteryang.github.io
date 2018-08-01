---
layout: post
title:  "Fix an ESLint issue in Visual Studio Code"
date:   2018-08-01 09:21:00 -0600
categories: nodejs
---

Fix an ESLint issue in Visual Studio Code
===================================================================================

Today when I started my asp.net core angular project in VS Code, I noticed on the status bar there was an ESLint error icon. Switching to ESLint debug console, I got the following error:
	
    Failed to load plugin react: Cannot find module 'eslint-plugin-react'

At first it looks quite strange as my project is not a react application. Why ESLint server in VS Code was loading a react plugin? I went to [ESLint configuration page](https://eslint.org/docs/user-guide/configuring) to have a look. And then I realized that it must be a global configuration was loaded. So I went to my home folder /Users/cyang, there is a .eslintrc file with this plugin setting in it:
```
  "plugins": [
    "react"
  ],
```
I think I have to overwrite it in my project by add this to my project package.json file

```
  "eslintConfig":{
    "plugins": [ ]
  }
```
After restarting VS Code, ESLint error is gone.