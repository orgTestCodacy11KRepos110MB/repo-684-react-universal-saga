## Introduction

### Getting Started

Universal React App Starter featuring Redux Saga. Heavily modified version of [React Redux Universal Hot Example](https://github.com/erikras/react-redux-universal-hot-example) combined with Redux Saga [real-world](https://github.com/yelouafi/redux-saga/tree/master/examples/real-world) example.

`$ git clone https://github.com/xkawi/react-universal-saga`

`$ npm install`

`$ npm run dev` (run in development mode)

### Libraries Used 

* [Universal](https://medium.com/@mjackson/universal-javascript-4761051b7ae9) rendering, with Client and Server Side Data Fetching
* [React](https://github.com/facebook/react)
* [React Router](https://github.com/rackt/react-router)
* [Express](http://expressjs.com)
* [Babel](http://babeljs.io) for ES6 and ES7 magic
* [Webpack](http://webpack.github.io) for bundling
* [Webpack Dev Middleware](http://webpack.github.io/docs/webpack-dev-middleware.html)
* [Webpack Hot Middleware](https://github.com/glenjamin/webpack-hot-middleware)
* [Redux](https://github.com/rackt/redux)'s futuristic [Flux](https://facebook.github.io/react/blog/2014/05/06/flux.html) implementation
* [Redux Dev Tools](https://github.com/gaearon/redux-devtools) for next generation DX (developer experience). Watch [Dan Abramov's talk](https://www.youtube.com/watch?v=xsSnOQynTHs).
* [ESLint](http://eslint.org) to maintain a consistent code style
* [redux-form](https://github.com/erikras/redux-form) to manage form state in Redux
* [style-loader](https://github.com/webpack/style-loader), [sass-loader](https://github.com/jtangelder/sass-loader) and [less-loader](https://github.com/webpack/less-loader) to allow import of stylesheets in plain css, sass and less,
* [bootstrap-sass-loader](https://github.com/shakacode/bootstrap-sass-loader) and [font-awesome-webpack](https://github.com/gowravshekar/font-awesome-webpack) to customize Bootstrap and FontAwesome
* [react-helmet](https://github.com/nfl/react-helmet) to manage title and meta tag information on both server and client
* [webpack-isomorphic-tools](https://github.com/halt-hammerzeit/webpack-isomorphic-tools) to allow require() work for statics both on client and server
* [mocha](https://mochajs.org/) to allow writing unit tests for the project.
* Refer to `package.json` for more details

## New to React?

Core Concepts to learn if you are new to ReactJS and this repo:

- Dump Components (Component) 
- Smart Component (Container)
- Actions - define available action types
- Reducers - given previous state and an action, create a new state
- Sagas - manage or orchestrate event/action dispatching flow (called Side Effects)

### Crash Course on React, Redux, and Redux Saga

1. [Hands-on] [Get started with ReactJS](https://facebook.github.io/react/docs/tutorial.html)

2. [Reading] [Thinking in React way](https://facebook.github.io/react/docs/thinking-in-react.html)

3. [Reading] [Flux Pattern](https://code-cartoons.com/a-cartoon-guide-to-flux-6157355ab207#3c82)
[(video)](https://www.youtube.com/watch?v=nYkdrAPrdcw&feature=youtu.be)

4. [Hand-on] [Flux](https://facebook.github.io/flux/docs/todo-list.html)

5. [Reading] [Why someone created a tool called Redux](https://code-cartoons.com/hot-reloading-and-time-travel-debugging-what-are-they-3c8ed2812f35)

6. [Reading] [More about Redux](https://code-cartoons.com/a-cartoon-intro-to-redux-3afb775501a6)

7. [Hands-on] [Getting started with Redux](https://egghead.io/series/getting-started-with-redux)

8. [Hands-on] [More comprehensive hands-on with Redux and React](http://teropa.info/blog/2015/09/10/full-stack-redux-tutorial.html)

9. [Reading] [Redux best practices](https://medium.com/lexical-labs-engineering/redux-best-practices-64d59775802e#.v34syy9ia)

10. [Reading + Hands-on] [Redux Saga](http://yelouafi.github.io/redux-saga/)

11. [*Reading - optional*] [Relay](https://code-cartoons.com/a-cartoon-intro-to-facebook-s-relay-part-1-3ec1a127bca5)

## Development Workflow

Here is a suggested development workflow that works for me:

1. Define the routes, which helps to reason about the needed containers and components.

2. Implement dump components + props (state and actions).
	* Each component should define `.propTypes` for prop's validation, which will throw a warning if the container that uses it do not provides all the props that the component need.
	
3. Often, a container = a route page; a container is also where you import necessary components, actions and states (pass down to component) to form a single page.
	* if a container wants to "pre-populate" data during server rendering process, assign `Container.preload` with sagas that fetches necessary data from the API server. (refer to `containers/App.js` for an example)

4. Implement necessary Actions as you define needed containers and components (PS: this implementation is independent from previous steps). It often helps maintainability and readability if action methods are categorized into 2:
    * Actions that manipulate store directly through reducers (e.g. `actions/index.js > createRequestTypes` method), often to update the state and trigger re-render the components. 
    * "LOAD" or "TRIGGER" actions for starting a saga routine/daemon, often to make network call.

5. Implement Reducers. As you implemented the actions, you will have better idea on how to modify the states for different actions. 

### Using Redux DevTools

[Redux Devtools](https://github.com/gaearon/redux-devtools) are enabled by default in development.

- <kbd>CTRL</kbd>+<kbd>H</kbd> Toggle DevTools Dock
- <kbd>CTRL</kbd>+<kbd>Q</kbd> Move DevTools Dock Position
- see [redux-devtools-dock-monitor](https://github.com/gaearon/redux-devtools-dock-monitor) for more detailed information.

If you have the
[Redux DevTools chrome extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd) installed it will automatically be used on the client-side instead.

If you want to disable the dev tools during development, set `__DEVTOOLS__` to `false` in `/webpack/dev.config.js`.
DevTools are not enabled during production.

## Deployment

`$ npm run build`

`$ npm run start`

## How Everything Works?

What initially gets run is `bin/server.js`, which does little more than enable ES6 and ES7 awesomeness in the
server-side node code. It then initiates `server.js`. In `server.js` we perform data fetching using Redux Saga ([details](https://github.com/yelouafi/redux-saga/issues/13#ref-commit-3e8321c)). Aside from serving the favicon and static content from `/static`, the only thing `server.js` does is initiate delegate rendering to `react-router`. At the bottom of `server.js`, we listen to port `3000` and initiate the API server.

### Routing and HTML return

The primary section of `server.js` generates an HTML page with the contents returned by `react-router`. Then we perform [server-side data fetching](#server-side-data-fetching), wait for the data to be loaded, and render the page with the now-fully-loaded `redux` state.

The last interesting bit of the main routing section of `server.js` is that we swap in the hashed script and css from the `webpack-assets.json` that the Webpack Dev Server – or the Webpack build process on production – has spit out on its last run. You won't have to deal with `webpack-assets.json` manually because [webpack-isomorphic-tools](https://github.com/halt-hammerzeit/webpack-isomorphic-tools) take care of that.

We also spit out the `redux` state into a global `window.__data` variable in the webpage to be loaded by the client-side `redux` code.

### Server-side Data Fetching

The [redux-saga](https://github.com/yelouafi/redux-saga) provides a mechanism for server-side data fetching from the actual backend API servers/services, so on client-side (React) there is no need to do additional network call. You have to define the Sagas that a container may need (refers to `containers/App.js > App.preload` for example), so that server-side only fetches data that is needed by the container/component. PS: You have the flexibility to implement additional logic (e.g. handle authentication) when fetching data at server-side rendering stage, as it differs from client-side. 

### Client Side

The client side entry point is reasonably named `client.js`. All it does is load the routes, initiate `react-router`, rehydrate the redux state from the `window.__data` passed in from the server, and render the page over top of the server-rendered DOM. This makes React enable all its event listeners without having to re-render the DOM.

### Redux Middleware

Currently, we only use Saga Middleware and Logger Middleware (for development). If you need to use or add custom middlewares, you can do so by modifying `store/configureStore.dev.js` (for dev env) or `store.configureStore.prod.js` (for prod env).

## Styling

### Local Styles

This project uses [local styles](https://medium.com/seek-ui-engineering/the-end-of-global-css-90d2a4a06284) using [css-loader](https://github.com/webpack/css-loader). The way it works is that you import your stylesheet at the top of the `render()` function in your React Component, and then you use the classnames returned from that import. Like so:

`render() { const styles = require('./App.scss'); }`

Then you set the `className` of your element to match one of the CSS classes in your SCSS file, and you're good to go!

`<div className={styles.mySection}> ... </div>`

### Global Style Variables

`react-universal-saga` support global style variables by defining the variable in `theme/style.scss`. Once defined, you can use in any scss file so long it is imported (refer to `RepoPage.scss` for example). 

## Unit Tests

The project uses [Mocha](https://mochajs.org/) to run your unit tests, it uses [Karma](http://karma-runner.github.io/0.13/index.html) as the test runner, it enables the feature that you are able to render your tests to the browser (e.g: Firefox, Chrome etc.), which means you are able to use the [Test Utilities](http://facebook.github.io/react/docs/test-utils.html) from Facebook api like `renderIntoDocument()`.

To run the tests in the project, just simply run `npm test` if you have `Chrome` installed, it will be automatically launched as a test service for you.

To keep watching your test suites that you are working on, just set `singleRun: false` in the `karma.conf.js` file. Please be sure set it to `true` if you are running `npm test` on a continuous integration server (travis-ci, etc).

## TODO

- [ ] Use Proxy Server for better performance and security
- [ ] Simple authentication saga flow
- [ ] Provide a more realistic example that has navigation and form
- [ ] Upgrade to React ^15
- [ ] Use [Jest](https://facebook.github.io/jest/) for unit testing