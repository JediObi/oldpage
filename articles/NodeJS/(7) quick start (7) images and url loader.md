### **(7) quick start (7) images and url loader**


+ ### [quick start (1) init project](https://www.jianshu.com/p/b5df2e74aa20)
+ ### [quick start (2) webpack config to compile javascript](https://www.jianshu.com/p/71e4b19c1264)
+ ### [quick start (3) html pages with javascript](https://www.jianshu.com/p/8e2656d51037)
+ ### [quick start (4) webpack-dev-server](https://www.jianshu.com/p/58dd29b62500)
+ ### [quick start (5) css](https://www.jianshu.com/p/e98d4c4d34cf)
+ ### [quick start (6) react](https://www.jianshu.com/p/9b31cb59ecb5)
+ ### quick start (7) images and url loader
+ ### [quick start (8) loaders and hash](https://www.jianshu.com/p/64fe50f2d3ad)
+ ### [quick start (9) a project of react](https://www.jianshu.com/p/395b299fa8f0)
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
│   │   └── test.jpg
│   └── index.js
├── webpack.config.dev.js
└── webpack.config.js
```

### (2) url loader
+ url-loader
1) help webpack process images during building.
2) change image url in js and css to build path.
```
<img src='./images/test.png'/> -> <img src='./static/media/test.png'/>
```
+ install
```
~:npm install --save-dev url-loader file-loader
```
url-loader depends on file-loader.

+ webpack config
webpack.config.js
```js
...
module: {
        loaders: [
            { test: /\.css$/, loader: "style-loader!css-loader" },
            { 
                test: /\.(js|jsx)$/, 
                loader: "babel-loader",
                query:
                {
                    presets:['react']
                },
            },
            {
                test: [/\.bmp$/, /\.gif$/, /\.jpe?g$/, /\.png$/],
                //loader: require.resolve('url-loader'),
                loader: 'url-loader',
                options: {
                  limit: 10000,
                  name: 'static/media/[name].[ext]',
                },
            },
        ]
    },
...
```
```limit: 10000```, images less than 10000 bytes will return a data URI instead of a path.

### (3) images
+ App.js
import xxx from './xxx/xxx.ext';
```js
import React, { Component } from 'react';
import './App.css'
import test_jpg from './img/test.jpg';

class App extends Component{

    render(){
        return (
            <div>
            <img src={test_jpg} alt='back'/>
            </div>
        );
    }
}

export default App;
```

+ in css
```css
body{
    /* background-image: url(./img/test.jpg); */
}
```

### (4) build project
+ build
```~:npm run build```
+ debug
```~:npm start```