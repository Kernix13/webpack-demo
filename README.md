# Webpack Setup Demo

Colt Steele Webpack Demo 2019

Check out his repo: [webpack-demo-app](https://github.com/Colt/webpack-demo-app)

## Video 1

- Webpack bundles many files into a smaller number of files
- It also manages dependencies

## Video 2

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

```sh
git init
git add .
git commit -m "First commit"
git branch -M main
git remote add origin https://github.com/Kernix13/webpack-demo.git
git push -u origin main
```
