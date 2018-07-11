### **(4) quick start (4) webpack-dev-server**


+ ### [quick start (1) init project](https://www.jianshu.com/p/b5df2e74aa20)
+ ### [quick start (2) webpack config to compile javascript](https://www.jianshu.com/p/71e4b19c1264)
+ ### [quick start (3) html pages with javascript](https://www.jianshu.com/p/8e2656d51037)
+ ### quick start (4) webpack-dev-server
+ ### [quick start (5) css](https://www.jianshu.com/p/e98d4c4d34cf)
+ ### [quick start (6) react](https://www.jianshu.com/p/9b31cb59ecb5)
+ ### [quick start (7) images and url loader](https://www.jianshu.com/p/30cf1c8bb2b1)
+ ### [quick start (8) loaders and hash](https://www.jianshu.com/p/64fe50f2d3ad)
+ ### [quick start (9) a project of react](https://www.jianshu.com/p/395b299fa8f0)
+ ### [react-helmet customize html head](https://www.jianshu.com/p/97ced0c8f891)

The following steps will lead you create http server for development.
Here we use ```webpack-dev-server``` to build it for ```Hot Module Replacement``` and ```automatic refresh```.

### install webpack-dev-server
```~:npm install --save-dev webpack-dev-server```

### config in webpack.config.js
```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry:  path.resolve(__dirname,'src/index.js'),
    output: {
        path: path.resolve(__dirname, 'build/static/js'),
        filename: 'bundle.js'
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style-loader!css-loader" }
        ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            inject: true, 
            template: path.resolve(__dirname, 'public/index.html'),
            filename: path.resolve(__dirname,'build/index.html'),
            minify:{
                removeAttributeQuotes: true,
                collapseWhitespace: true,
                removeRedundantAttributes: true,
                useShortDoctype: true,
                removeEmptyAttributes: true,
                removeStyleLinkTypeAttributes: true,
                keepClosingSlash: true,
                minifyJS: true,
                minifyCSS: true,
                minifyURLs: true,
            }
        }),
    ],
    devServer: {
        contentBase: path.resolve(__dirname, 'public'),
        historyApiFallback: false,
        inline:true,
        port:3000,
    },
}
```
```contentBase```: specify the content base, the server will serve the static files in this directory.
```historyApiFallback```: if your project is SPA, make it true.
```inline```: automatic refresh and hot module replacement.
### node script
+ package.json
```json
{
  "name": "lesson1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack-dev-server",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "html-webpack-plugin": "^2.30.1",
    "webpack": "^3.10.0",
    "webpack-dev-server": "^2.9.7"
  }
}
```
```~:npm start```
### Javascripts haven't been injected.
### multiple webpack config files
```webpack --config webpack.config.js```
Webpack could specify a config file for compiling.
We've created some scripts in ```package.json``` to run ```webpack```.
Now we create a new config file for webpack-dev-server so solve the injection problem.
+ webpack.config.dev.js
```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');


module.exports = {
    entry:  path.resolve(__dirname,'src/index.js'),
    output: {
        path: path.resolve(__dirname, 'build/static/js'),
        filename: 'bundle.js'
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style-loader!css-loader" }
        ]
    },
    plugins:[
        new HtmlWebpackPlugin({
            inject: true, //inject all js at the bottom of the body
            template: path.resolve(__dirname, 'public/index.html'), //source file
        }),
    ],
    devServer: {
        contentBase: path.resolve(__dirname, 'public'),
        historyApiFallback: false,
        inline:true,
        port:3000,
    },
}
```
+ package.json
```json
{
  "name": "lesson1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "start": "webpack-dev-server --config webpack.config.dev.js"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "html-webpack-plugin": "^2.30.1",
    "webpack": "^3.10.0",
    "webpack-dev-server": "^2.9.7"
  }
}
```
```~:npm start```
open http://localhost:3000

