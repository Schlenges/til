# React Router
```npm install --save react-router-dom```

Routing, or the conditional rendering of components based on the url in the browser, is used by placing components as children of the Router (imported as Router from BrowserRouter) component, meaning inside Router-tags. BrowserRouter is a Router that uses the HTML5 history API (pushState, replaceState and the popState event) to keep your UI in sync with the URL.
Inside the router we define links that modify the address bar with the help of the Link component.
```JS
<Link to="/notes">notes</Link>
```
Components rendered based on the URL of the browser are defined with the help of the component Route. For example:
```JavaScript
<Route path="/notes" render={() => <Notes />} />
```
defines that if the address in the browser is /notes, then the component Notes is rendered.

For the root address / we have to use the modifier *exact* in front of the path attribute of the route. Otherwise the home component is also rendered on all other paths, since the root / is included at the start of all other paths.

## Parameterized route
```JS
const noteById = (id) =>
  notes.find(note => note.id === Number(id))

<Route exact path="/notes/:id" render={({ match }) =>
  <Note note={noteById(match.params.id)} />
} />
```
The render attribute, which defines the component to be rendered, can access the id using its parameter called match.  
A match object contains information about how a ```<Route path>``` matched the URL. match objects contain the following properties:  
  **params** - (object) Key/value pairs parsed from the URL corresponding to the dynamic segments of the path  
  **isExact** - (boolean) true if the entire URL was matched (no trailing characters)  
  **path** - (string) The path pattern used to match. Useful for building nested <Route>s  
  **url** - (string) The matched portion of the URL. Useful for building nested <Link>s

## withRouter and history
```JS
import {
  // ...
  withRouter} from 'react-router-dom'

const LoginNoHistory = (props) => {
  const onSubmit = (event) => {
    event.preventDefault()
    props.onLogin('mluukkai')
    props.history.push('/')  }

  return (
    <div>
      <h2>login</h2>
      <form onSubmit={onSubmit}>
        <div>
          username: <input />
        </div>
        <div>
          password: <input type='password' />
        </div>
        <button type="submit">login</button>
      </form>
    </div>
  )
}

const Login = withRouter(LoginNoHistory)
```
There are a few notable things about the implementation of the form. When logging in, we call the function onSubmit, which calls a method called push of the history-object received by the component as a prop. The command props.history.push('/') results in the address bar of the browser changing its address to / thereby making the application render the respective component, which in this case is Home.
The component gets access to the history-prop after it is "wrapped" by the function withRouter.


# Webpack
Even though ES6 modules are defined in the ECMAScript standard, no browser actually knows how to handle code that is divided into modules. For this reason, code that is divided into modules must be bundled for browsers, meaning that all of the source code files are transformed into a single file that contains all of the application code. Under the hood, the npm script bundles the source code using webpack.  
In practice, bundling is done so that we define an entry point for the application, which typically is the index.js file. When webpack bundles the code, it includes all of the code that the entry point imports, and the code that its imports import, and so on. Since part of the imported files are packages like React, Redux, and Axios, the bundled JavaScript file will also contain the contents of each of these libraries.
```JS
const path = require('path')

const config = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'main.js'
  }
}
module.exports = config
```
The output property defines the location where the bundled code will be stored. The target directory must be defined as an absolute path which is easy to create with the path.resolve method. We also use __dirname which is a global variable in Node that stores the path to the current directory.

## Bundling React
Next, let's transform our application into a minimal React application. Let's install the required libraries:
```npm install --save react react-dom```  
We still need the build/index.html file that will serve as the "main page" of our application that will load our bundled JavaScript code with a script tag.  
By default, webpack only knows how to deal with plain JavaScript. Although we may have become unaware of it, we are actually using JSX for rendering our views in React. We can use **loaders** to inform webpack of the files that need to be processed before they are bundled. Let's configure a loader to our application that transforms the JSX code into regular JavaScript:
```JS
module: {
  rules: [
    {
      test: /\.js$/,
      loader: 'babel-loader',
      query: {
        presets: ['@babel/preset-react'],
      },
    },
  ],
},
```
Loaders are defined under the module property in the rules array. The definition for a single loader consists of three parts:  
- The test property specifies that the loader is for files that have names ending with .js.
- The loader property specifies that the processing for those files will be done with babel-loader
- The query property is used for specifying parameters for the loader, that configure its functionality.  

Let's install the loader and its required packages as a development dependency: ```npm install @babel/core babel-loader @babel/preset-react --save-dev```

It's worth noting that if the bundled application's source code uses async/await, the browser will not render anything on some browsers. Googling the error message in the console will shed some light on the issue. We have to install one more missing dependency, that is **@babel/polyfill**. Let's make the following changes to the entry property of the webpack configuration object in the webpack.config.js file:
```JS
entry: ['@babel/polyfill', './src/index.js']
```

## Babel
The transpilation process that is executed by Babel is defined with plugins. In practice, most developers use ready-made presets that are groups of pre-configured plugins. Currently we are using the @babel/preset-react preset for transpiling the source code of our application. Let's add the @babel/preset-env plugin that contains everything needed to take code using all of the latest features and transpile it to code that is compatible with the ES5 standard.   
When using CSS, we have to use css and style loaders:
```JS
{
  test: /\.css$/,
  loaders: ['style-loader', 'css-loader'],
}
```
The job of the css loader is to load the CSS files and the job of the style loader is to generate and inject a style element that contains all of the styles of the application. With this configuration the CSS definitions are included in the main.js file of the application. For this reason there is no need to separately import the CSS styles in the main index.html file of the application. If needed, the application's CSS can also be generated into its own separate file by using the mini-css-extract-plugin.


**Webpack-dev-server**  

The current configuration makes it possible to develop our application but the workflow is awful (to the point where it resembles the development workflow with Java). Every time we make a change to the code we have to bundle it and refresh the browser in order to test the code. The webpack-dev-server offers a solution to our problems. Let's define an npm script for starting the dev-server:
```JSON
{
  // ...
  "scripts": {
    "build": "webpack --mode=development",
    "start": "webpack-dev-server --mode=development"  },
  // ...
}
```
Let's also add a new devServer property to the configuration object in the webpack.config.js file:
```JS
const config = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'build'),
    filename: 'main.js',
  },
  devServer: {
    contentBase: path.resolve(__dirname, 'build'),
    compress: true,
    port: 3000,
  },
  // ...
};
```
The npm start command will now start the dev-server at the port 3000. When we make changes to the code, the browser will automatically refresh the page. The process for updating the code is fast. When we use the dev-server, the code is not bundled the usual way into the main.js file. The result of the bundling exists only in memory.

We will also ask webpack to generate a so-called **source map** for the bundle, that makes it possible to map errors that occur during the execution of the bundle to the corresponding part in the original source code (no extra package needed):
```JS
devtool: 'source-map',
```

## Minifying the code
The optimization process for JavaScript files is called minification. One of the leading tools intended for this purpose is UglifyJS. Starting from version 4 of webpack, the minification plugin does not require additional configuration to be used. It is enough to modify the npm script in the package.json file to specify that webpack will execute the bundling of the code in *production* mode.