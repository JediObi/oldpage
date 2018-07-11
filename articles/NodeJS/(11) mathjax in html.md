### **(11) mathjax in html**


I want to use formulas in my pages.
+ script environment
index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script type="text/javascript"
        src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
    </script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {inlineMath: [['$','$'], ['\(','\)']]}
        });
    </script>
</head>
<body>
    <div id='root'>
    </div>
</body>
</html>
```
+ latex in js
App.js
```js
import React, { Component } from 'react';
import './App.css'

class App extends Component{
    render(){
        return (            
            <div>
                $$\frac {1}{2} $$
            </div>
        );
    }
}

export default App;
```

+ development and build
development
```~:npm start```
build
```~:npm run build```