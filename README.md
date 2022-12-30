1. [Using Repo as Template](#using-repo-as-template)
2. [Generating this Template](#generating-this-template)
      1. [Create new Container](#create-new-container)
      2. [Instaling minimal packages](#instaling-minimal-packages)
      3. [NPM Helper function](#npm-helper-function)
      4. [Adding User same as in hostos](#adding-user-same-as-in-hostos)
      5. [Installing Minimal ( typescript + webpack )](#installing-minimal--typescript--webpack-)
      6. [Check generated file](#check-generated-file)
      7. [Generating tsconfig.json](#generating-tsconfigjson)
            1. [Webpack config file](#webpack-config-file)
      8. [Dummy Test code](#dummy-test-code)
      9. [Run webpack](#run-webpack)
            1. [Run webpack for Production build](#run-webpack-for-production-build)
            2. [Run webpack Debugging build](#run-webpack-debugging-build)
3. [Further links](#further-links)

* Sample setup for Nodejs + Webpack + Typescript *

## Using Repo as Template 
```bash
## Checkout from git
mkdir -p /home/hemant/Desktop/Node_Projects/typescript_webpack_01
cd /home/hemant/Desktop/Node_Projects/typescript_webpack_01
git clone https://git.affinity.com/hemantkumar.patel/node_setup.git .
## Removing .git to avoid accidental push 
rm -f .git
## Installing node packages
npm install
npx webpack
ls dist/
> dist/bundle.js.map
> dist/bundle.js
> 
```
  
## Generating this Template
#### Create new Container
```bash
docker run --name node4 --hostname node4 -v /home/hemant:/home/hemant:rw -it ubuntu:20.04 bash
```
  
>  #### *BELLOW ALL COMMANDS NOW INSIDE CONTAINER*
  
#### Instaling minimal packages
```bash
apt-get update
apt-get install nodejs npm vim -y
```
  
#### NPM Helper function
``` bash
function  npmll(){
## Helper dunftion to list all available version of package
echo  "Latest top 5 versions ";
npm view $1 versions --json | tail -n 6 | awk -vpkg="${1}" -F '"'  '{print "npm i -D " pkg "@" $2 }'
echo  "Currenlty installed version ";
npm list $1;
  
echo -ne "\n\nRemoving eisting package\n"
echo  "npm uninstall ${1}";
echo  "";
}
npmll tsc
```
  
#### Adding User same as in hostos
```bash
useradd hemant
mkdir -p /home/hemant
chown hemant:hemant /home/hemant
```
  
##Init new Node project
```bash
npm init --y
```
  
#### Installing Minimal ( typescript + webpack )
```bash
npm install --save-dev \
typescript \
ts-loader \
webpack \
webpack-cli \
wrapper-webpack-plugin \
declaration-bundler-webpack-plugin \
copy-webpack-plugin \
clean-webpack-plugin \
@types/node \
@types/webpack \
@types/google-publisher-tag
```
  
#### Check generated file
vim package.json
  
#### Generating tsconfig.json
```bash
vim tsconfig.json
{
"compilerOptions": {
"outDir": "./dist/",
"noImplicitAny": true,
"module": "es6",
"target": "es5",
"jsx": "react",
"allowJs": true,
"moduleResolution": "node",
"sourceMap": true
}
}
```
###### Webpack config file
```bash
vim webpack.config.js
```
  
```javascript
const  path = require('path');
const  WrapperPlugin = require('wrapper-webpack-plugin');
  
module.exports = {
entry:  './src/index.ts',
module: {
rules: [
{
test: /\.tsx?$/,
use:  'ts-loader',
exclude: /node_modules/,
},
],
},
resolve: {
extensions: ['.tsx', '.ts', '.js'],
},
plugins: [
// strict mode for the whole bundle
new  WrapperPlugin({
test: /\.js$/, // only wrap output of bundle files with '.js' extension
header:  '(function (W,D,N) { "use strict";W[N]=W[N]||{};if(W[N].hb23){return;}W[N].hb23="20221228";\n',
footer:  '\n})(window,document,"__afflib");'
})
],
mode:  "production",
output: {
chunkLoadingGlobal:  'AffChunk',
chunkLoading:  'jsonp',
filename:  'bundle.js',
path:  path.resolve(__dirname, 'dist'),
// libraryTarget: 'amd',
// library: "AffHb2023",
}
};
```
  
#### Dummy Test code
```bash
mkdir src
echo  "export function add( a:number , b:number ) : number { return a + b ; } " > src/lib.ts
echo -ne "import { add } from './lib'
console.log( add(1,3) )
" > src/index.ts
```
#### Run webpack
###### Run webpack for Production build
######## Also source map enaled in production, you can utilise it
```bash
npx webpack
cat dist/bundle.js
cat dist/bundle.js.map
```
  
###### Run webpack Debugging build
```bash
npx webpack --mode development watch
npx webpack watch
```
  
  
## Further links
* https://khalilstemmler.com/blogs/typescript/node-starter-project/
* https://gist.github.com/rupeshtiwari/e7235addd5f52dc3e449672c4d8b88d5
* https://webpack.js.org/guides/getting-started/
* https://www.typescriptlang.org/docs/
* [Online Gitlab Markdown Editor](https://stackedit.io/app##using-this-repo-as-tempalte)
