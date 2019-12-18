
## Webpack

**reference**

https://webpack.docschina.org/

**install**

```
npm i -D webpack webpack-cli
```

**build**

```
npx webpack --config webpack.config.js
```

of

> package.json

```
  "scripts": {
    "build": "webpack --config webpack.config.js"
  },
```


## Babel Loader

**Reference**

https://github.com/babel/babel-loader


**install**

```
npm install -D babel-loader @babel/core @babel/preset-env webpack
```

**Use**

> webpack.config.js
> 
```
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]
}
```

or

```
{
  entry: 'index.js',
  output: {
    path: __dirname + '/dist',
    filename: 'index_bundle.js'
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: 'My App',
      filename: 'assets/admin.html'
    })
  ]
}
```

## jQuery

**install**

```
npm install jquery
```

## html-webpack-plugin

**Reference**

https://webpack.docschina.org/plugins/html-webpack-plugin/#%E5%AE%89%E8%A3%85

https://github.com/jantimon/html-webpack-plugin


**install**

```
npm install --save-dev html-webpack-plugin
```

**Use**

> webpack.config.js

```
var HtmlWebpackPlugin = require('html-webpack-plugin');
var path = require('path');

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js'
  },
  plugins: [new HtmlWebpackPlugin()]
};

```




