### **(12) open browser after webpack compile**


+ use plugin ```open-browser-webpack-plugin```
+ install
```~:npm install --save-dev open-browser-webpack-plugin```

+ webpack.config.dev.js
```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const OpenBrowserPlugin = require('open-browser-webpack-plugin');

const host_port = 3000;

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
        new OpenBrowserPlugin({
            url: 'http:localhost:'+host_port,
        }),
    ],
    devServer: {
        contentBase: path.resolve(__dirname, 'public'),
        historyApiFallback: false,
        inline:true,
        port:host_port,
    },
}
```
+ package.json scripts
```json
"scripts": {
     "start": "webpack-dev-server --config webpack.config.dev.js"
  },
```

+ run 
```~:npm start```