# 基于electron-react-redux的桌面程序制作
本文章将要讲述怎么将electron，react，redux结合起来，当中react+redux的结合很简单，主要是与electron的结合
还有与webpack配合以及热加载的配置

有一个我自己做的[demo](https://github.com/zhangzewei/electron-react-redux-demo)也许会帮助到你

## 步骤

1. 首先我们需要建立一个新的文件夹 `mkdir my-electron-react-app && cd my-electron-react-app`;
2. 然后我们初始化一个 `package.json` ，`npm init`
3. 将一下代码复制进刚才创建的 `package.json` 中

      ```json
        {
          "name": "xxx",
          "version": "1.0.0",
          "description": "",
          "main": "index.js",
          "devDependencies": {
            "babel": "^5.6.10",
            "babel-core": "^5.6.11",
            "babel-eslint": "7.1.1",
            "babel-loader": "^5.2.2",
            "css-loader": "^0.15.1",
            "electron": "^1.4.1",
            "electron-packager": "^4.1.3",
            "electron-rebuild": "^0.2.2",
            "less": "^2.5.1",
            "less-loader": "^2.2.0",
            "node-libs-browser": "^0.5.2",
            "prop-types": "^15.5.7",
            "style-loader": "^0.12.3",
            "webpack": "^1.9.12",
            "webpack-dev-server": "^1.9.0"
          },
          "dependencies": {
            "electron-prebuilt": "^0.28.3",
            "react": "^15.5.0",
            "react-dom": "^15.5.0",
            "react-redux": "^4.4.5",
            "redux": "^3.5.2"
          },
          "scripts": {
            "test": "npm test",
            "start": "./node_modules/electron-prebuilt/dist/Electron.app/Contents/MacOS/Electron .",
            "watch": "./node_modules/.bin/webpack-dev-server",
            "electron-rebuild": "./node_modules/.bin/electron-rebuild"
          },
          "author": "xxx",
          "license": "ISC"
        }
      ```
4. 创建一个`webpack.config.js` 文件

      ```js
        var webpack = require('webpack');
        module.exports = {
            entry: {
            app: ['webpack/hot/dev-server', './app/index.js'],
          },
          output: {
            path: './public/built',
            filename: 'bundle.js',
            publicPath: 'http://localhost:8080/built/'
          },
          devServer: {
            contentBase: './public',
            publicPath: 'http://localhost:8080/built/'
          },
          module: {
          loaders: [
            { test: /\.js$/, loader: 'babel-loader', exclude: /node_modules/ },
            { test: /\.css$/, loader: 'style-loader!css-loader' },
            { test: /\.less$/, loader: 'style-loader!css-loader!less-loader'}
          ]
          },
          plugins: [
            new webpack.HotModuleReplacementPlugin()
          ]
        }
      ```

5. 然后我们需要在根目录创建一个index.js的文件

      ```js
        var app = require('app');
        var BrowserWindow = require('browser-window');
        require('crash-reporter').start();
        app.on('window-all-closed', function() {
          if (process.platform != 'darwin') {
            app.quit();
          }
        });
        app.on('ready', function() {
          mainWindow = new BrowserWindow({width: 900, height: 500});
          mainWindow.loadUrl('file://' + __dirname + '/public/index.html');
          mainWindow.openDevTools();
          mainWindow.on('closed', function() {
            mainWindow = null;
          });
        });
      ```
6. 然后我们创建一个public文件夹，在文件夹下创建一个index.html文件

      ```html
        <!DOCTYPE html>
        <html>
          <head>
            <title>My Electron app</title>
            <meta charset="utf-8">
            <meta name="viewport" content="width=device-width, initial-scale=1">
          </head>
          <body>
            <div id="root"></div>
          </body>
          <script src="http://localhost:8080/webpack-dev-server.js"></script>
          <script src="http://localhost:8080/built/bundle.js"></script>
        </html>
      ```
7. 最后我们需要创建一个app文件夹，然后在app文件夹中创建一个index.js文件

      ```js 
        import './less/main.less';
        import React from 'react';
        import ReactDOM from 'react-dom';
        import App from './App';
        import { Provider } from 'react-redux';
        import configureStore from './reducers/';

        const store = configureStore();

        ReactDOM.render(
          <Provider store={store}>
            <App />
          </Provider>,
          document.getElementById('root')
        );
      ```
8. 在这个app文件夹中就是我们的react+redux组合所在的地方，等你将一个简单的react+redux的项目做好之后，就能够在命令窗口输入命令了

      ```
        npm install
        npm watch
        // 重新打开一个命令窗口之后找到文件夹
        npm start
      ```