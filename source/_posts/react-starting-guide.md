---
title: React Starting Guide with material-ui
date: 2016-05-27 16:20:24
tags:
---
I've been struggling for hours on how to start a project in <REACT-JS-URL>, with the beautiful <MATERIAL-UI-URL>. So I wanted to share in a simple way the fastest way to get started.

## Dependencies

Okay first thing i had to learn was react isn't just another library like <JQUERY-URL>, it uses ES6 and with that it carries a bunch of concepts and tools you are gonna need.

the tools are : 

1. Nodejs / npm
2. wepack
3. babel

### Nodejs / npm

You should know you way into node already if you dont then head to <NODEJS-URL> basically it's the platform where everything resides within and provides a way to download the packages you need in your project. Im not going into detail on how to install it, just follow the official webpage instructions and you should be on your way.

### WebPack

Webpack is in charge of making a bundle that a browser can understand, it puts together css, html, javascript, images, etc.

### Babel

As i mentioned before, react works in ES6 which browsers does not currently implement, Babel transforms this unsupported features into "normal" javascript that the browser can understand.

## Files needed

Just two basic configuration files are needed, one for NPM and one for webpack






``` javascript hello	

var x = 5;  
var y = 2;
var z = x * y;
document.getElementById("demo").innerHTML = z;
```