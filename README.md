# Webpack Boilerplate

A [Webpack 4](https://webpack.js.org/) boilerplate with build-in:

- HTML file generation to serve your webpack bundles using [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin)
- ECMAScript 6 to ECMAScript 5 transpiling with [babel](https://babeljs.io/)
- CSS extraction into a single file using [style-loader](https://github.com/webpack-contrib/style-loader), [css-loader](https://github.com/webpack-contrib/css-loader) and [css-mini-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin)
- SCSS support using [sass-loader](https://github.com/webpack-contrib/sass-loader) and [node-sass](https://github.com/sass/node-sass).
- Image and font imports with [file-loader](https://github.com/webpack-contrib/file-loader)
- Github Pages publishing using [gh-pages](https://www.npmjs.com/package/gh-pages)

## Get Started

- [Project Structure](#project-structure)
- [Commands](#commands)
  - [Development](#development)
  - [Production](#production)
  - [Deploy to Github Pages](#deploy-to-github-pages)
- [Setup](#setup)
  - [Quick setup](#quick-setup)
  - [Complete setup](#complete-setup)
    - [Create your package.json and customize it](#create-your-packagejson-and-customize-it)
    - [Install Webpack](#install-webpack)
    - [Create files](#create-files)
    - [Add HTML to your generated Bundle](#add-html-to-your-generated-bundle)
    - [Transplate your JS with Babel](#transplate-your-js-with-babel)
    - [Styling: import and inject CSS](#styling-import-and-inject-css)
    - [Import images](#import-images)
    - [Optimize CSS and Javascript assets](#optimize-css-and-javascript-assets)
    - [Use Bootstrap](#use-bootstrap)
    - [Use FontAwesome](#use-fontawesome)
    - [Deploy to Github Pages](#deploy-to-github-pages)
    - [Use aliases](use-aliases)
    - [Use BrowserSync](use-brosersync)

## Project Structure

```
Project
│   README.md
│   package.json
│   webpack.config.js
└───src
│   │   index.html
│   └───scripts
│   |   └───index.js
│   └───scss
│       └───main.scss
└───dist

```

## Setup

1. Clone this repository into a new project folder (replace `[project name]` with your project's name)

   ```
   git clone git@github.com:DigitalCareerInstitute/dci-boilerplate-II.git [project name]
   ```

1. Delete the boilerplate's git history

   ```
   cd <project name>
   rm -rf .git
   ```

1. Edit `package.json` to add you project's name

   `package.json`

   ```json
   {
     "name": "[project name]",
     ...
     "author": "[your name]"
   }
   ```

1. Edit `index.html` to add your projects name

   ```html
   ...
   <head>
     ...
     <title>[project name]</title>
   </head>
   ...
   ```

1. Start a new git repository and make an initial commit

   ```
   git init
   git add . && git commit -m "Initial commit"
   ```

1. Install the dependencies

   ```
   npm install
   ```

1. Have fun coding :D

## Useful Commands

### Development

Run **Webpack** in **Development** mode and start coding!

```
npm start
```

### Production

Run **Webpack** in **Production** mode.

```
npm run build
```

### Deploy to Github Pages

Deploy your code to **Github Pages**: this script creates a 'gh-pages' branch and serve the production bundle to this branch

```
npm deploy
```

## DIY Tutorial

The following sections will guide you in configuring your own boilerplate from scratch.

### Create your package.json and customize it

```

npm init

```

### Initialize Webpack

1. Install Webpack

   ```
   npm i -D webpack webpack-cli webpack-dev-server
   ```

1. Setup your development scripts in `package.json`

   ```json
   {
     ...
     "scripts": {
     "start": "webpack-dev-server --mode development",
     "build": "webpack --mode production",
     }
     ...
   }
   ```

1. Create a basic configuration file in `webpack.config.js`

   ```javascript
   const path = require('path');

   const config = {
     entry: './src/scripts/index.js',
     output: {
       path: path.resolve(__dirname, 'dist/'),
       filename: path.join('scripts', 'bundle.js')
     }
   };

   module.exports = config;
   ```

### Test it out

1. Create a file in `src/scripts/index.js` and insert some starting JS code there.

   ```javascript
   let message = 'Hello Webpack';
   console.log(` Message is: ${message}`);
   ```

1. Run the dev server and open a browser window on `localhost:8080`

   ```
   npm start
   ```

   You **should** see the message when you open the console.

### Transpile your ESNext with Babel

1. Install [Babel](https://babeljs.io/) to transpile your code down to ES5

   ```
   npm i -D @babel/core babel-loader @babel/preset-env
   ```

1. Add the following configuration to your `webpack.config.js`

   ```javascript
   //...

   const config = {
     //...
     module: {
       rules: [
         // Scripts
         {
           test: /\.js$/,
           exclude: /node_modules/,
           use: {
             loader: 'babel-loader',
             options: {
               presets: ['@babel/preset-env'],
               sourceMaps: true
             }
           }
         }
       ]
     }
   };

   //...
   ```

### Test it out

1. Run the build script


    ```
    npm run build
    ```

    You **should** see a javascript file in `dist/scripts/bundle.js`

### Add HTML to your generated Bundle

1. Install [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin) to generate an `index.html`

   ```
   npm i -D html-webpack-plugin html-loader
   ```

1. Create a basic template for your HTML page in `src/index.html`

   ```html
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0" />
       <meta http-equiv="X-UA-Compatible" content="ie=edge" />
       <title>[project title]</title>
     </head>
     <body>
       <h1>Hello from src</h1>
     </body>
   </html>
   ```

1. Add the following configuration to your `webpack.config.js`

   ```javascript
   //at the beginning of the file
   const HtmlWebpackPlugin = require('html-webpack-plugin');

   module.exports = {
     //...
     module: {
       rules: [
        // Scripts
        {
          //...
        },
        // index.html
        {
          test: /\.html$/,
          use: [
            {
              loader: 'html-loader',
              options: { minimize: true }
            }
          ]
        }
      ]
     },
     plugins: [
        new HtmlWebpackPlugin({
          template: 'index.html',
          filename: './index.html'
        })
      ];
    }
   ```

#### Test it out

1. Run the build script


    ```
    npm run build
    ```

    You **should** see an html file file in `dist/index.html`

1. Open `dist/index.html` in the browser

   You **should** see the heading you added to the template displayed on the page.

   You **should** see the message from the js file when you open the console.

### Adding some style

### Working with Images and Fonts

### Source maps and production

### Supporting GithubPages Deployment

### Bonus Tasks: Bootstrap and Fontawesome

---

### Create starter files

1. Create a file in **/src/scripts/index.js** and insert some starting JS code there.

   ```javascript
   let message = 'Hello Webpack';
   console.log(` Message is: ${message}`);
   ```

1. Create a file in **/src/styles/main.scss** and insert some starting SCSS styles there.

   ```scss
   $font-stack: Helvetica, sans-serif;
   $primary-color: #bb2b2b;

   body {
     font: 100% $font-stack;
     color: $primary-color;
     text-align: center;
   }

   img {
     max-width: 500px;
   }
   ```

1. Add a `logo.png` image in **/src/img/logo.png**

#### Styling: import and inject CSS

To import and use CSS styles we need to add a [style-loader](https://github.com/webpack-contrib/style-loader) and [css-loader](https://github.com/webpack-contrib/css-loader). Css-loader will import content to a variable and style-loader will inject content into the HTML file as an inline tag. To support SCSS we also need to add [sass-loader](https://github.com/webpack-contrib/sass-loader) and [node-sass](https://github.com/sass/node-sass).

```
npm i -D style-loader css-loader sass-loader node-sass
```

and add the configuration for the loaders to your **webpack.config.js**

```javascript
// in the configuration -> module -> rules

    //style and css loader
    {
        test: /\.css$/,
        use: ["style-loader", "css-loader", 'sass-loader']
    }

```

Extracting all CSS into a single file

Styles are now injected as an inline. We will extract styles using [css-mini-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin) and we'll move the styles to an external stylesheet file.
This stylesheet will be then injected into the index.html automatically.

```
npm i -D mini-css-extract-plugin
```

and add the configuration for css-mini-extract-plugin to your **webpack.config.js**

```javascript

// at the beginning of the file
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

// in the configuration -> module -> rules
      {
        test: [/.css$|.scss$/],
        use: [
          MiniCssExtractPlugin.loader,
          "css-loader",
          "sass-loader"
        ]
      }

// in the configuration -> plugins

module.exports = {

...

  plugins: [
    ...
    new MiniCssExtractPlugin({
      filename: "assets/css/styles.css"
    }),
    ...
  ]
```

#### Import images

To include images we need to configure [file-loader](https://github.com/webpack-contrib/file-loader)

```
npm i -D file-loader
```

and add the configuration for file-loader to your **webpack.config.js**

```javascript
// in the configuration -> module -> rules

    //file loader
    {
        test: /\.(png|jpg|gif|svg)$/,
        use: [
          {
            loader: 'file-loader',
            options: {
              name: '[name].[ext]',
              outputPath: 'assets/'
            }
          }
        ]
    }

```

#### Optimize CSS and Javascript assets

We want to optimize the webapp by minifying our assets with [uglifyjs-webpack-plugin](https://github.com/webpack-contrib/uglifyjs-webpack-plugin) and [optimize-css-assets-webpack-plugin](https://github.com/NMFR/optimize-css-assets-webpack-plugin).
Note: Webpack 4 optimizes JS bundle by default when using **production** mode.

```
npm i -D uglifyjs-webpack-plugin optimize-css-assets-webpack-plugin
```

and add the configuration to your **webpack.config.js**

```javascript
// at the beginning of the file
const UglifyJsPlugin = require("uglifyjs-webpack-plugin");
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");

// in the configuration -> optimization

module.exports = {
...

  optimization: {
    minimizer: [
      new UglifyJsPlugin(),
      new OptimizeCSSAssetsPlugin()
    ]
  },

```

#### Use Bootstrap

For Bootstrap to compile, we need to you install and use the required loaders: postcss-loader with autoprefixer.

```
npm i -D postcss-loader autoprefixer
```

and add the configuration to your **webpack.config.js**

```javascript


// in the configuration -> module -> rules change

      //style and css extract
      {
        test: [/.css$|.scss$/],
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader", {
          loader: 'postcss-loader',
          options: {
            plugins: () => [require('autoprefixer')({
              'browsers': ['> 1%', 'last 2 versions']
            })],
          }
        }]
      },

```

We can now install Bootstrap module

```
npm i bootstrap @fortawesome/fontawesome-free
```

In **/src/assets/js/index.js** import Bootstrap

```javascript
...
import 'bootstrap';
...

```

In **/src/assets/styles/styles.scss** import Bootstrap

```scss
...
@import "custom";
@import "~bootstrap/scss/bootstrap";
...

```

Create a new file In **/src/assets/styles/custom.scss** and move your style there

```scss
$theme-colors: (
  'primary': #666969
);

$font-stack: Helvetica, sans-serif;

body {
  font: 100% $font-stack;
  color: $primary-color;
  text-align: center;
}

img {
  max-width: 500px;
}
```

#### Use Fontawesome

Install fontawesome

```
npm i @fortawesome/fontawesome-free
```

````
In  **/src/assets/styles/styles.scss** set the path and import FA

```scss
...
$fa-font-path: '~@fortawesome/fontawesome-free/webfonts';
@import '~@fortawesome/fontawesome-free/scss/fontawesome';
@import '~@fortawesome/fontawesome-free/scss/regular';
@import '~@fortawesome/fontawesome-free/scss/solid';
@import '~@fortawesome/fontawesome-free/scss/brands';
...

````

and add the configuration to your **webpack.config.js**

```javascript

// in the configuration -> module -> rules

      //fonts
      {
        test: /\.(woff(2)?|ttf|eot|svg)(\?v=\d+\.\d+\.\d+)?$/,
        use: [{
          loader: 'file-loader',
          options: {
            name: '[name].[ext]',
            outputPath: 'assets/fonts/',
            publicPath: '../fonts'
          }
        }]
      },

```

#### Deploy to Github Pages

We want to publish files to a new branch (called gh-pages) on GitHub using the [gh-pages](https://www.npmjs.com/package/gh-pages) module.

```
npm i -D gh-pages
```

In **package.json** scripts add

```javascript
  "deploy": "npm run build && gh-pages -d dist",
```

This script help us to create a **gh-pages** branch on Github and also serve our bundled files on that branch.

#### Use Aliases

Using alias we'll simplify the imports.
Add the configuration to your **webpack.config.js**

```javascript
resolve: {
    alias: {
      '@scss': path.resolve(__dirname, 'src/assets/scss'),
      '@img': path.resolve(__dirname, 'src/assets/img'),
      '@': path.resolve(__dirname, 'src')
    }
  }
```

Edit **/src/assets/js/index.js** and change

```javascript
import '@scss/styles.scss';
import logoImg from '@img/logo.png';

let filename = logoImg.substring(logoImg.lastIndexOf('/') + 1);
logo.src = `assets/img/${filename}`;
```

#### Use BrowserSync

BrowserSync will start only when you run Webpack in watch mode.

```javascript
npm i -D browser-sync-webpack-plugin browser-sync
```

and add the configuration to your **webpack.config.js**

```javascript
//at the beginning of the file
const BrowserSyncPlugin = require('browser-sync-webpack-plugin');

//in the configuration -> plugins
plugins: [
  ...new BrowserSyncPlugin({
    host: 'localhost',
    port: 3000,
    server: { baseDir: ['dist'] }
  })
];
```

---

### Credits

Quality metadata badges from [shields.io](https://shields.io)  
Background image from [thepatternlibrary](http://thepatternlibrary.com/#fancy-pants)
