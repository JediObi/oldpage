### **(9) quick start (9) a project of react**


+ ### [quick start (1) init project](https://www.jianshu.com/p/b5df2e74aa20)
+ ### [quick start (2) webpack config to compile javascript](https://www.jianshu.com/p/71e4b19c1264)
+ ### [quick start (3) html pages with javascript](https://www.jianshu.com/p/8e2656d51037)
+ ### [quick start (4) webpack-dev-server](https://www.jianshu.com/p/58dd29b62500)
+ ### [quick start (5) css](https://www.jianshu.com/p/e98d4c4d34cf)
+ ### [quick start (6) react](https://www.jianshu.com/p/9b31cb59ecb5)
+ ### [quick start (7) images and url loader](https://www.jianshu.com/p/30cf1c8bb2b1)
+ ### [quick start (8) loaders and hash](https://www.jianshu.com/p/64fe50f2d3ad)
+ ### quick start (9) a project of react
+ ### [react-helmet customize html head](https://www.jianshu.com/p/97ced0c8f891)

### (1) directory tree
```
.
├── package.json
├── package-lock.json
├── public
│   └── index.html
├── README.md
├── src
│   ├── App.css
│   ├── App.js
│   ├── img
│   │   ├── logo.svg
│   │   └── test.jpg
│   └── index.js
├── webpack.config.dev.js
└── webpack.config.js
```

### (2) package.json
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
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.2",
    "babel-preset-react": "^6.24.1",
    "css-loader": "^0.28.7",
    "extract-text-webpack-plugin": "^3.0.2",
    "file-loader": "^1.1.6",
    "html-webpack-plugin": "^2.30.1",
    "react": "^16.2.0",
    "react-dom": "^16.2.0",
    "style-loader": "^0.19.1",
    "url-loader": "^0.6.2",
    "webpack": "^3.10.0",
    "webpack-dev-server": "^2.9.7"
  }
}
```

### (3) webpack.config.js
```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
    entry:  path.resolve(__dirname,'src/index.js'),
    output: {
        path: path.resolve(__dirname, 'build/'),
        filename: 'static/js/[name].[chunkhash:8].js'
    },
    module: {
        rules:[
            {
                oneOf:[
                    { 
                        test: /\.(js|jsx)$/, 
                        loader: 'babel-loader',
                        query:
                        {
                            presets:['react']
                        },
                    },
                    { 
                        test: /\.css$/, 
                        loader: "style-loader!css-loader" },
                    
                    {
                        test: [/\.bmp$/, /\.gif$/, /\.jpe?g$/, /\.png$/],
                        //loader: require.resolve('url-loader'),
                        loader: 'url-loader',
                        options: {
                          limit: 10000,
                          name: 'static/media/[name].[hash:8].[ext]',
                        },
                    },
                    {
                        exclude: [/\.js$/,/\.html$/,/\.json$/],
                        //loader: require.resolve('url-loader'),
                        loader: 'file-loader',
                        options: {
                          name: 'static/media/[name].[hash:8].[ext]',
                        },
                    },

                ],
            },
        ],
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
        new ExtractTextPlugin({
            filename: 'static/css/[name].[contenthash:8].css',
        }),
    ],
}
```

### (4) webpack.config.dev.js
```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry:  path.resolve(__dirname,'src/index.js'),
    output: {
        path: path.resolve(__dirname, 'build/'),
        filename: 'static/js/bundle.js'
    },
    module: {
        rules:[
            {
                oneOf:[
                    { 
                        test: /\.(js|jsx)$/, 
                        loader: 'babel-loader',
                        query:
                        {
                            presets:['react']
                        },
                    },
                    { 
                        test: /\.css$/, 
                        loader: "style-loader!css-loader"
                    },                    
                    {
                        test: [/\.bmp$/, /\.gif$/, /\.jpe?g$/, /\.png$/],
                        //loader: require.resolve('url-loader'),
                        loader: 'url-loader',
                        options: {
                          limit: 10000,
                          name: 'static/media/[name].[hash:8].[ext]',
                        },
                    },
                    {
                        exclude: [/\.js$/,/\.html$/,/\.json$/],
                        //loader: require.resolve('url-loader'),
                        loader: 'file-loader',
                        options: {
                          name: 'static/media/[name].[hash:8].[ext]',
                        },
                    },

                ],
            },
        ],
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

### (5) code
+ #### index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id='root'></div>
</body>
</html>
```

+ #### index.js
```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
    <App />, document.getElementById('root')
);
```

+ #### App.js
```js
import React, { Component } from 'react';
import './App.css'
import logo from './img/logo.svg';

class App extends Component{

    render(){
        return (
            <div>
            <img src={logo} alt='logo'/>
            </div>
        );
    }
}

export default App;
```

+ #### App.css
```css
body{
    background-image: url(./img/test.jpg);
}
```
