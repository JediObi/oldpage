### **(1) quick start (1) init project**


+ ### quick start (1) init project
+ ### [quick start (2) webpack config to compile javascript](https://www.jianshu.com/p/71e4b19c1264)
+ ### [quick start (3) html pages with javascript](https://www.jianshu.com/p/8e2656d51037)
+ ### [quick start (4) webpack-dev-server](https://www.jianshu.com/p/58dd29b62500)
+ ### [quick start (5) css](https://www.jianshu.com/p/e98d4c4d34cf)
+ ### [quick start (6) react](https://www.jianshu.com/p/9b31cb59ecb5)
+ ### [quick start (7) images and url loader](https://www.jianshu.com/p/30cf1c8bb2b1)
+ ### [quick start (8) loaders and hash](https://www.jianshu.com/p/64fe50f2d3ad)
+ ### [quick start (9) a project of react](https://www.jianshu.com/p/395b299fa8f0)
+ ### [react-helmet customize html head](https://www.jianshu.com/p/97ced0c8f891)

the following steps will lead you create a new node project.
the final tree
```
.
└── package.json

```
 ## init a project
```
~:mkdir lesson1
~:cd lesson1
~:npm init
```
```npm init``` will generate ```package.json``` in lesson1/. 
+ package.json
```json
{
  "name": "lesson1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```
```package.json``` file manages the javascript packages or modules like maven pom.xml. Now add ```webpack``` to our project , ```name``` and ```version``` specification of it like the followting code:
+ command
```
~:npm install --save-dev webpack
```
+ package.json
```json
{
  "name": "lesson1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^3.10.0"
  }
}
```
+ this is the develop dependencies
```json
"devDependencies": {
    "webpack": "^3.10.0"
}
```
```--save```: if we don't use this command, we have to add the dependencies manually. This command will add packages to ```dependencies```.
```--save-dev```: it's similar to ```--save```, but it will add packages to ```devDependencies```.
dev means we just use these packages in development, and in our final product, some of the packages we don't need such like webpack, and some will be compiled into a more advanced file such like jQuery. 
