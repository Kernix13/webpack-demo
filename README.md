# Webpack Setup Demo

Colt Steele Webpack Demo 2019

Check out his repo: [webpack-demo-app](https://github.com/Colt/webpack-demo-app) and the [Webpack main documention](https://webpack.js.org/concepts/).

I stopped coding along when he started using `template.html` or `html-webpack-plugin`, plus I was not able t o use SASS either - something about `dart-sass` so this video series was a bust on some practical code but I did get some more insights into using Webpack. Also, I don't think I would split up my config file like he did - I like th common object and then an if statement for `dev` or `prod`. Here are my notes:

## Video 1

- Webpack bundles many files into a smaller number of files nd manages dependencies

## Installing and Running Webpack

- [Learn Webpack Pt. 2: Installing and Running Webpack and Webpack-CLI](https://youtu.be/5XrYSbUbS9o)
- FIRST: break the starter code into separate files and then add script files for them
- but the order is important and in a large app that is difficult

### Install Weback

- Create `package.json`: `npm init -y` or without `-y`
- Install Webpack: `npm install -D webpack webpack-cli` or replace `-D` with `--save-dev`
- Set up `scripts` in `package.json`: use `start` and set it to `webpack` so running `npm start` in the command line will call Webpack
- you can run that cmd right now but you will run into problems - before we configure webpack, ...
- by default, when you install Webpack and you don't have a `.config.js` file, it has a couple of default values one of which is `index.js` in `./src` which is the default entry point
- so create a file in `src` with that name - if you run `npm start` now you will get a message about a file name `main.js` and a warning about not setting the mode
- Okay, I got a lot of errors -

## Imports Exports and Webpack Modules

Video: [Learn Webpack Pt. 3: Imports, Exports, & Webpack Modules](https://youtu.be/8QYt1_17nk8)

- import files
- what is going in `index.js` - it's our entry-point -
- check the commit `Webpack now bundling all our app code` - may need to download and copy all of that
- End of Commit 4: https://github.com/Colt/webpack-demo-app/commit/2b11dd3624422ac8f57fced592dd824230c83693

## Configuring Webpack

Video: [Learn Webpack Pt. 4: Configuring Webpack](https://youtu.be/ZwWiDZoPMB0)

- without a config fie, Webpack is looking for an entry point in `src/index.js` - the only thing we did was create a `start` script
- also by default, Webpack is putting the bundled code in `dist/main.js`
- to change any of that or to add things, you need a config file - he wants one for dev and for prod, but for now just one - name it `webpack.config.js`, although he says you can name it whatever you want
- the first line should be `module.exports = {` and he added `entry` and `output`
- for the `path` property we need to import/require a module from Node.js called `path` then: `path: path.resolve(__dirname, "dist")`
- that "resolves" an ABSOLUTE path to that code directory - doing `pwd` for me resulted in `/c/Users/14844/Documents/WebDev/Steele/webpack-demo` - but if anyone contributes or clones your repo, you need it to be an absolute path for thier machine or for the path when live.
- the last step is to tell Webpack to use this file:

```json
"scripts": {
    "start": "webpack --config webpack.config.js"
  },
```

- besides `dist`, it's common to see `build` or `public`
- the problem with `dist/main.js` is that the code is minified because it is running in `production` mode by default so add a `mode` prop and set it to `development`
- more readable but still messed up IMO - it is also using `eval()` throughout main.js - to change that add `devtool: "none",` - NOPE, that caused an error - check out https://webpack.js.org/configuration/devtool/
- use `devtool: false,`

## Loaders CSS and SASS

Video: [Learn Webpack Pt. 5: Loaders, CSS, & SASS](https://youtu.be/rrMGUnBmjwQ)

- JSON, CSS, SASS, SVG, etc - loaders!!!
- Loaders are needed for different file types other than JS - they are used to pre-process diff types of files and are really useful
- create `src/main.css` - you could include a link to it in your html fle but you want to use webpack -
- we need 2 different loaders: `style-loader` and `css-loader`
- `npm install --save-dev style-loader css-loader`
- css-loader is to turn CSS into valid JS code but the CSS is not applied
- style-loader takes the JS for the CSS from css-loader ad injects it into the DOM
- the order is css-loader then style-loader but they load in reverse order so that is why style-loader is first in the array in the config file
- later we will create a separate CSS file - commit so we can change it to load SASS files - here is the CSS Webpack rules:

```js
module: {
  rules: [
    {
      test: /\.css$/,
      use: ["style-loader", "css-loader"]
    }
  ];
}
```

- right now we are pulling Bootstrap in from a CDN but instead install it locally with `npm install --save-dev bootstrap` - remove the CDN link
- remove the styles in main.css and rename it with .scss - then import bootstrap into that file
- but now we need a loader for SASS: `sass-loader` - he mentions `node-sss` but the new command is `npm install sass-loader sass --save-dev`
- WTF you need dart-sass or something - MOTHER FUCKING CAN'T USE SASS AGAIN

## Cache Busting and Plugins

Video: [Learn Webpack Pt. 6: Cache Busting and Plugins](https://youtu.be/qXRGKiHmtF8)

- how to prevent assets (JS, CSS) files cached by browsers when we don't want them to be cahced
- add a content hash into the file name so users get changes - if nothing changes in the file, the hash doesn't change, it the code changes we get a new file name -> cache busting
- he changed the config file to `filename: "main.[contenthash].js",` but webpack has `filename: '[name].[contenthash].js',`
- but how do you include that script file in the html - have Webpack build the HTML for us and add it to the dist folder - you need a plugin for that
- search for `contenthash` on Webpack's site

### Plugins

- we need `HtmlWebpackPlugin` and we have to clean up the dist folder
- `npm install --save-dev html-webpack-plugin`
- add a key of plugins in the config file but rememeber to also require it in index.js
- it will create a new html file: `dist/index.html` with the hashed JS file in a script tag (in the `<head>` tag though???)
- from webpack:

> "You can either let the plugin generate an HTML file for you, supply your own template using lodash templates, or use your own loader"

- inside `src` create `template.html` - think this is wrong - old video
- he passed in an object in the plugin function call

## Splitting Dev and Production

Video: [Learn Webpack Pt. 7: Splitting Dev & Production](https://youtu.be/VR5y93CNzeA)

- 3 config files: cmmon, dev, prod
- no more coding from here because I'm not creating `template.html`
- create `webpack.dev.js` and `webpack.prod.js` and change config file to `webpack.common.js`

`webpack.common.js`:

```js
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: "./src/index.js",
  plugins: [new HtmlWebpackPlugin()]
};
```

`webpack.dev.js`:

```js
const path = require("path");
const common = require("./webpack.common");
const merge = require("webpack-merge");

module.exports = merge(common, {
  mode: "development",
  output: {
    filename: "main.js",
    path: path.resolve(__dirname, "dist")
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  }
});
```

`webpack.prod.js`:

```js
const path = require("path");
const common = require("./webpack.common");
const merge = require("webpack-merge");

module.exports = merge(common, {
  mode: "production",
  output: {
    filename: "main.[contenthash].js",
    path: path.resolve(__dirname, "dist")
  }
});
```

- then you need the package `npm install --save-dev webpack-merge`
- the commands are `npm start` and `npm run build`
- then install `npm install --save-dev webpack-dev-server`
- then change package.json:

```json
"scripts": {
    "start": "webpack-dev-server --config webpack.dev.js --open",
    "build": "webpack --config webpack.prod.js"
  },
```

- `--open` is an optional flag to open the file in the browser
- `rm -rf dist` to remove the dist folder

## HTML Loader File and Loader and Clean Webpack

Video: [Learn Webpack Pt. 8: Html-loader, File-loader, & Clean-webpack](https://youtu.be/mnS_1lolc44)

- IMAGES: we need all our assets in the dist folder - we need to copy them into that folder and hash the file names - move the assets folder into `src` if it is not already there
- we need 2 packages: html-loader and file-loader - we need to require in the images and then handle images in the webpack file
- `npm install --save-dev html-loader file-loader` - then in webpack.common.js add them to rules:

```js
rules: [
  {
    test: /\.html$/,
    use: ["html-loader"]
  },
  {
    test: /\.{jpg|jpeg|png|gif|svg}$/,
    use: {
      loader: "file-loader",
      options: {
        name: "[name].[hash].[ext]",
        outputPath: "imgs"
      }
    }
  }
];
```

- the reason for the `options` object is the hash the file name
- looks like you don't have to use `require()` for images
- Clean Webpack Plugin: `npm install --save-dev clean-webpack-plugin`
- in webpack.prod.js require it in then pass it into a plugins value
- looks like plugins get the `new` keyword and parens

## Multiple Entrypoints and Vendor JS

Video: [Learn Webpack Pt. 9: Multiple Entrypoints & Vendor.js](https://youtu.be/PT0znBWIVnU)

- to separate out app code from vendor code like Bootstrap and/or jQuery
- they will never change but your code will
- set up a new entry point - create `src/vendor.js` and inside add `import "bootstrap";`
- then in the common config file, change entry to:

```js
entry: {
    main: "./src/index.js",
    vendor: "./src/vendor.js"
  },
```

- in t he prod file refactor `filename` in t he `output` object to `filename: "[name].[contenthash].bundle.js",` and in the dev file change it to `filename: "[name].bundle.js",`
- note for Bootstrap js you also need to install 2 dependencies: jquery and popper.js

## Extract CSS and Minify

Video: [Learn Webpack Pt. 10: Extract CSS & Minify HTML/CSS/JS](https://youtu.be/JlBDfj75T3Y)

- Flash Of Unstyled Content - need a separate CSS file
- in development you don't want to wait to recompile and bundle CSS files
- `npm install --save-dev mini-css-extract-plugin` though I don't think it's a `devDependencies` - then require it into the prod file and we need to use it instead of style-loader which is in the common file

```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin")

module: {
    rules: [
      {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"]
      }
    ]
  },

plugins: [new MiniCssExtractPlugin({filename: [name].[contenthash].css}), new CleanWebpackPlugin()]
```

- but the file is not minified - he is using a plugin named `optimize-css-assets-webpack-plugin` but maybe try `CssMinimizerWebpackPlugin`
- terser-webpack-plugin is needed for JS because of using the above plugin

> NEED A DAMN RECENT SERIES ON HOW TO USE WEBPACK!!!!!!!!!!
