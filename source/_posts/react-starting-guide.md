---
title: Simple React Starting Guide with material-ui
date: 2016-05-27 16:20:24
tags:
---
I've been struggling for hours on how to start a project in {% link React https://facebook.github.io/react/ %}, with the beautiful {% link material-ui http://www.material-ui.com/#/ %}. So I wanted to share a simple and fast way to get started. This guide was created using the explanations given in {% link survivejs https://survivejs.com %}, so if you have the time this is the ultimate tutorial guys I really recommend it.

## Dependencies

Okay first thing I had to learn was react isn't just another library like {% link jquery https://jquery.com/ %}, it uses ES6 and with that it carries a bunch of concepts and tools you are gonna need:

1. Nodejs / npm
2. Webpack
3. Babel

### Nodejs / npm

You should know your way into node already, if you don't then head to {% link the nodejs oficial website https://nodejs.org/en/ %}. Basically it's the platform where everything resides and provides a way to download the packages you need in your project. Im not going into detail on how to install it, just follow the official webpage instructions and you should be on your way.

### WebPack

Webpack is in charge of making a bundle that a browser can understand, it puts together css, html, javascript, images, etc.

### Babel

As I mentioned before, react works in ES6 which browsers does not currently implement, Babel transforms this unsupported features into "normal" javascript that the browser can understand.

## Configuration Files needed

Just three basic configuration files are needed, one for npm (Nodejs package manager), one for webpack and one for babel. I will put the code below and hopefully the comments will be self-explanatory.


``` javascript package.json	
{
  "name": "appName",
  "version": "1.0.0",
  "description": "App description",
  "main": "index.js",
  //these are executed by running "npm run <script>"
  "scripts": {
    //script to run webpack by npm
    "build": "webpack", 
    //starts the webpack webserver
    "start": "webpack-dev-server --content-base build" 
  },
  "keywords": [],
  "author": "hugosama",
  "license": "ISC",
  /**
  * these dependencies are created either by adding them here or runnin npm 
  * --save-dev or -D, the difference betweeen these and "dependencies" is that npm 
  * does not install the dev-dependencies of the packages or any documentation
  **/
  "devDependencies": {
    "babel-core": "^6.7.7",
    "babel-loader": "^6.2.4",
    "babel-preset-es2015": "^6.6.0",
    "babel-preset-react": "^6.5.0",
    "css-loader": "^0.23.1",
    "npm-install-webpack-plugin": "^3.1.0",
    "react-hot-loader": "^1.3.0",
    "style-loader": "^0.13.1",
    "webpack": "^1.13.0",
    "webpack-dev-server": "^1.14.1"
  },
  "dependencies": {
    "material-ui": "^0.14.4",
    "react": "^15.0.2",
    "react-dom": "^15.0.2"
  }
}
```

``` javascript webpack.config.js
const path = require('path');
var webpack = require('webpack');

const NpmInstallPlugin = require('npm-install-webpack-plugin');


const PATH = {
    app: path.join(__dirname,'app'),
    build: path.join(__dirname,'build')
};

module.exports =  {
    devServer: {
        contentBase: PATH.build,
        // Enable history API fallback so HTML5 History API based
      // routing works. This is a good default that will come
      // in handy in more complicated setups.
        historyApiFallback: true,
        hot:true,
        inline: true,
        progress: true,
      // Display only errors to reduce the amount of output.
      status: 'errors-only',
            // Parse host and port from env so this is easy to customize.
      // 0.0.0.0 is available to all network devices unlike default
      // localhost
      host: process.env.HOST,
      port: process.env.PORT
    },
    // Add resolve.extensions.
    // '' is needed to allow imports without an extension.
    // Note the .'s before extensions as it will fail to match without!!!
    resolve: {
        extensions: ['', '.js', '.jsx']
    },
     plugins: [
      new webpack.HotModuleReplacementPlugin(),
      new NpmInstallPlugin({
        save: true // --save
      })
    ],
    devtool: 'eval-source-map',
     // Entry accepts a path or an object of entries.
    entry: {
        app: PATH.app
    },
    output: {
        path: PATH.build,
        filename: 'bundle.js'
    },
    module: {
        //these loaders remove the app need to reload in order to load changes
        loaders: [
            {
                test:/\.css$/,
                loaders: ['style', 'css'],
                // Include accepts either a path or an array of paths.
                include: PATH.app
            },
            {
                test: /\.jsx?$/,
                loaders: ['react-hot','babel?cacheDirectory'],
                include: PATH.app
            }
        ]
    }
};
```

``` javascript .babelrc
//these tells babel which presets are we using to compile our javascript
{
  "presets": [
    "es2015",
    "react"
  ]
}

```


With these three configuration files we are all set to make an awesome app with {% link React https://facebook.github.io/react/ %} and {% link material-ui http://www.material-ui.com/#/ %}.

## Coding the Actual Application

The configuration we made previously expects the following scaffolding : 
```
.
+-- app
|   +-- index.jsx
+-- build
|   +-- index.html
|   +-- bundle.js
+-- node_modules
+-- .babelrc
+-- package.json
+-- webpack.config.js
```
As you can see we are missing three files: index.jsx, index.html and bundle.js. Let's fix that, here is the index.html file : 

``` HTML index.html
<!DOCTYPE html>
<html>
<head>
    <title>Sample App</title>
</head>
<body>
<div id="app"></div>
<script type="text/javascript" src="bundle.js"></script>
</body>
</html>
```
And here is a hello world like code for react with material ui, this goes in the index.jsx file : 

``` javascript index.jsx 
import React from 'react';
import ReactDom from 'react-dom';
import AppBar from 'material-ui/lib/app-bar';

class App extends React.Component {
    render() {
        return (   
          <AppBar
            title="Hello World"
            iconClassNameRight="muidocs-icon-navigation-expand-more"
          />
        );
    } 
}

ReactDom.render(<App/>,document.getElementById('app'));
```
Now the only missing file is build.js, this is a special file generated by webpack, it's going to be our last step to greatness!

## Last step

The last step in this simple tutorial is to execute three commands in the console : 
```
npm install
```
then
```
npm run build
```
and
```
npm start
```

Finally we head to http://localhost:8080 and we shall see something like this:

![](/images/hello-world.png "Hello World Image")

which is an Appbar component from {% link material-ui http://www.material-ui.com/#/ %}.

## Conclusion

This post was intended as a super simple I want to get started right now guide into react with material-ui. The configuration files should help start any new project and hopefully will get you going real quick.