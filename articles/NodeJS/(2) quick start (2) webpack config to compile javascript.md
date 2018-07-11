### **(2) quick start (2) webpack config to compile javascript**


+ ### [quick start (1) init project](https://www.jianshu.com/p/b5df2e74aa20)
+ ### quick start (2) webpack config to compile javascript
+ ### [quick start (3) html pages with javascript](https://www.jianshu.com/p/8e2656d51037)
+ ### [quick start (4) webpack-dev-server](https://www.jianshu.com/p/58dd29b62500)
+ ### [quick start (5) css](https://www.jianshu.com/p/e98d4c4d34cf)
+ ### [quick start (6) react](https://www.jianshu.com/p/9b31cb59ecb5)
+ ### [quick start (7) images and url loader](https://www.jianshu.com/p/30cf1c8bb2b1)
+ ### [quick start (8) loaders and hash](https://www.jianshu.com/p/64fe50f2d3ad)
+ ### [quick start (9) a project of react](https://www.jianshu.com/p/395b299fa8f0)
+ ### [react-helmet customize html head](https://www.jianshu.com/p/97ced0c8f891)

The following steps will show you how to use webpack.
We will configure ```webpack.config.js``` and compile source files to build the project. 
The final tree
```
.
├── package.json
├── package-lock.json
├── src
│   └── index.js
└── webpack.config.js
```
### webpck compile
#### (1) code js
+ index.js
```js
console.log('Hello, this is a test!');
```
 #### (2) compile js
We did [init project](p/b5df2e74aa20) and installed ```webpack```.
```npm install``` just install a package in current project directory.
```webpack``` can be used as a command if it was installed in global.
+ install global
```~:npm install -g webpack```
+ use webpack to compile
```
~:webpack abc.js bundle.js
```
It will compile abc.js and create bundle.js. We use bundle.js in index.html.
### webpack.config.js
 We have too many compiling ruls with different source files and options.
So we have to simplify the webpack command rule.

And then we can just use ```~:webpack``` to instead of ```~:webpack abc.js bundle.js```.

Put all webpack compiling rule in config file:

+ create ```webpack.config.js```
```js
const path = require('path');

module.exports = {
    entry: [
        './index.js'
    ],
    output: {
        path: path.resolve(__dirname, 'build/static/js'),
        filename: 'bundle.js'
    }
}
```
```entry```: webpack will analyze your entry file for dependencies to other files. These files (called modules) are included in your bundle.js too. webpack will give each module a unique id and save all modules accessible by id in the bundle.js file. Only the entry module is executed on startup. A small runtime provides the require function and executes the dependencies when required.
```output```: webpack will create lesson1/build/bundle.js. All modules will be included in this file.
### node script
+ package.json
```json
{
  "name": "lesson1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" &amp;&amp; exit 1",
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
```~:npm run build```
+ run bundle.js
```
~:cd build/static/js
~:node bundle.js
Hello, this is a test!
```