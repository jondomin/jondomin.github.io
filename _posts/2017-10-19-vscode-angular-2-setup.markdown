---
layout: post
title: Angular 2 Debugging in VSCode
date: '2017-10-19 18:28:53'
---

1. Install Chrome Debugger Extension
2. Create launch.json file
3. Copy the contents below into the configurations array of the launch.json file.
4. Start ng development server (ng serve)

<script src="https://gist.github.com/jldeveloper27/f5c3f4e29b7dda76ec297254701a8b9c.js"></script>

Press F5/Debug and you should now be able to debug the solution

## Install Chrome
If you experience this error `Can't find Chrome - install it or set the "runtimeExecutable"` it may be due to the fact that you don't have chrome installed.  Make sure you install Chrome browser on your machine if you want use this debugging method.
