### **(8) quick start (8) loaders and hash**


+ ### [quick start (1) init project](https://www.jianshu.com/p/b5df2e74aa20)
+ ### [quick start (2) webpack config to compile javascript](https://www.jianshu.com/p/71e4b19c1264)
+ ### [quick start (3) html pages with javascript](https://www.jianshu.com/p/8e2656d51037)
+ ### [quick start (4) webpack-dev-server](https://www.jianshu.com/p/58dd29b62500)
+ ### [quick start (5) css](https://www.jianshu.com/p/e98d4c4d34cf)
+ ### [quick start (6) react](https://www.jianshu.com/p/9b31cb59ecb5)
+ ### [quick start (7) images and url loader](https://www.jianshu.com/p/30cf1c8bb2b1)
+ ### quick start (8) loaders and hash
+ ### [quick start (9) a project of react](https://www.jianshu.com/p/395b299fa8f0)
+ ### [react-helmet customize html head](https://www.jianshu.com/p/97ced0c8f891)

### (1) loaders
previously on:
webpack can't process file if it is not a javascript file. So we must install different loaders for different types.

#### 1) loaders list
```js
babel-loader // for es6 and jsx language
css-loader style-loder // for css
url-loader // for images
file-loader // for other files, and url-loader depends on it.
```

#### 2) webpack.config.js
+ ##### original loaders in webpack.config.js
```js
...
module.exports = {
    entry:  ...,
    output: {...},
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
    plugins:[...],
}
```
we have to add a file-loader to process other files.
```js
{ 
      exclude: [/\.js$/,/\.html$/,/\.json$/],
      loader: 'file-loader',
      options: {
          name: 'static/media/[name].[ext]',
      },
},
```
But this loader may process images which will cause a confict to url-loader.
+ ##### new config
```js
...
module.exports = {
    entry:  ...,
    output: {...},
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
                          name: 'static/media/[name].[ext]',
                        },
                    },
                    {
                        exclude: [/\.js$/,/\.html$/,/\.json$/],
                        //loader: require.resolve('url-loader'),
                        loader: 'file-loader',
                        options: {
                          name: 'static/media/[name].[ext]',
                        },
                    },
                ],
            },
        ],
    },
    plugins:[...],
}
```
"oneOf" will traverse all following loaders until one will match the requirements. When no loader matches it will fall back to the "file" loader at the end of the loader list.

### (2) hash
#### 1) why use hash
Add hash code into file name. 
If a file content is changed, the next time to build it will change hash code in its name.  Browser's visit site will change which will cause a new request for this file. A long cache kept in browser if the hash code dosen't change.

#### 2) how to use hash
+ ##### production config
```js
...
module.exports = {
    entry:  ...,
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
                        ...
                    },
                    { 
                        test: /\.css$/, 
                        ...
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
    plugins:[...],
}
```
+ #### development config
```js
...
module.exports = {
    entry:  ...,
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
                        ...
                    },
                    { 
                        test: /\.css$/, 
                        ...
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
    plugins:[...],
    devServer: {...},
}
```
css file need a plugin to add hash.

+ #### hash for css file
install
```
~:npm install --save-dev extract-text-webpack-plugin
```
webpack.config.js
```js
...
const ExtractTextPlugin = require('extract-text-webpack-plugin');
...
plugins:[
       ...
        new ExtractTextPlugin({
            filename: 'static/css/[name].[contenthash:8].css',
        }),
    ],
...
```




