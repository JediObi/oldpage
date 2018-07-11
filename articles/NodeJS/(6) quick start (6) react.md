### **(6) quick start (6) react**


+ ### [quick start (1) init project](https://www.jianshu.com/p/b5df2e74aa20)
+ ### [quick start (2) webpack config to compile javascript](https://www.jianshu.com/p/71e4b19c1264)
+ ### [quick start (3) html pages with javascript](https://www.jianshu.com/p/8e2656d51037)
+ ### [quick start (4) webpack-dev-server](https://www.jianshu.com/p/58dd29b62500)
+ ### [quick start (5) css](https://www.jianshu.com/p/e98d4c4d34cf)
+ ### quick start (6) react
+ ### [quick start (7) images and url loader](https://www.jianshu.com/p/30cf1c8bb2b1)
+ ### [quick start (8) loaders and hash](https://www.jianshu.com/p/64fe50f2d3ad)
+ ### [quick start (9) a project of react](https://www.jianshu.com/p/395b299fa8f0)
+ ### [react-helmet customize html head](https://www.jianshu.com/p/97ced0c8f891)

### install modules
+ react
```~:npm install --save-dev react```
React core.

+ react-dom
```~:npm install --save-dev react-dom```
ReactDOM for root render.

+ babel-loader
```~:npm install --save-dev babel-loader```
Use jsx language in js or jsx. Must config in ```webpack.config.js```.

+ babel-core
```~:npm install --save-dev babel-core```
babel core

+ babel-preset-react
```~:npm install --save-dev babel-preset-react```
babel for react. Must config in ```webpack.config.js```.

### webpack config
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
            // css loader
            { test: /\.css$/, loader: "style-loader!css-loader" },
            // babel loader
            { 
                test: /\.(js|jsx)$/, 
                loader: "babel-loader",
                // babel preset react
                query:
                {
                    presets:['react']
                },
            },
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

### code js
+ index.js
```js
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import './index.css'

// require('./index.css')
// document.write("hello, this is a test!");
//console.log('Hello, this is a test!');
class App extends Component{
    render(){
        return (
            <div>
                First React DOM!
            </div>
        );
    }
}

ReactDOM.render(
    <App/>,
    document.getElementById('root')
);
```
```import React from 'react';``` every component needs to import react core module ```React```.
```import ReactDOM from 'react-dom';``` if you want to render a component in your html page you have to import ReactDOM and use it.

### run
+ command
```~:npm start```

