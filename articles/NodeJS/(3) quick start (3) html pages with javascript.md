### **(3) quick start (3) html pages with javascript**


+ ### [quick start (1) init project](https://www.jianshu.com/p/b5df2e74aa20)
+ ### [quick start (2) webpack config to compile javascript](https://www.jianshu.com/p/71e4b19c1264)
+ ### quick start (3) html pages with javascript
+ ### [quick start (4) webpack-dev-server](https://www.jianshu.com/p/58dd29b62500)
+ ### [quick start (5) css](https://www.jianshu.com/p/e98d4c4d34cf)
+ ### [quick start (6) react](https://www.jianshu.com/p/9b31cb59ecb5)
+ ### [quick start (7) images and url loader](https://www.jianshu.com/p/30cf1c8bb2b1)
+ ### [quick start (8) loaders and hash](https://www.jianshu.com/p/64fe50f2d3ad)
+ ### [quick start (9) a project of react](https://www.jianshu.com/p/395b299fa8f0)
+ ### [react-helmet customize html head](https://www.jianshu.com/p/97ced0c8f891)

The following steps will show you how to use webpack to compile html pages.
We have built ```bundle.js``` in [quick start (2) webpack config](/p/71e4b19c1264), now we use it in html pages.
The final tree without node_modules:
```
.
├── package.json
├── package-lock.json
├── public
│   └── index.html
├── src
│   └── index.js
└── webpack.config.js
```
### install html-webpack-plugin
This plugin will use your html pages source files as templates to create pages in buiding step.
```~:npm install --save-dev html-build-plugin```
### code sources
#### (1) make direcotry for public items
```
~:cd lesson1
~:mkdir public
```
#### (2) code html
+ index.html
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
    <div id="root"></div>
</body>
</html>
```
#### (3) code javascript
```js
// console.log('Hello, this is a test!');
document.write("hello, this is a test!");
```
### html config in webpack.config.js
This config will use public/index.html as a template to create build/index.html and inject ```bundle.js``` into build/index.html.
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
}
```
inject:```true``` means inject all js at the bottom of the <body>

### build project
+ command
```~:npm run build```
+ open build/index.html in browser