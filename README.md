# feathers-reduxify-authentication
Wrap feathers.authentication so it works transparently with Redux, as well as authentication, authorization packages for React-Router.

[![Build Status](https://travis-ci.org/eddyystop/feathers-reduxify-authentication.svg?branch=master)](https://travis-ci.org/eddyystop/feathers-reduxify-authentication)
[![Coverage Status](https://coveralls.io/repos/github/eddyystop/feathers-reduxify-authentication/badge.svg?branch=master)](https://coveralls.io/github/eddyystop/feathers-reduxify-authentication?branch=master)

- Work with standard `feathers.authentication` on the client.
- Dispatch feathers authentication to Redux.
- Integrate with `react-router` and `react-router-redux`.
- Use popular Redux, React-Router authentication and authorization packages such as
[redux-auth-wrapper](https://github.com/mjrussell/redux-auth-wrapper)

Transfering production code here. Wait for 0.1.0.

## Code Example

```javascript
const feathersReduxifyAuthentication = require('feathers-reduxify-authentication');
// Configure feathers-client
const app = feathers().configure(feathers.authentication({ ... });

// Reduxify feathers-authentication
feathersAuthentication = reduxifyAuthentication(app,
  { isUserAuthorized: (user) => user.isVerified } // we insist user is 'verified' to authenticate
);
// Add Redux reducer
combineReducers({ ..., auth: feathersAuthentication.reducer, ...});

// Dispatch actions as needed. Params are the same as for feathers.authentication().
dispatch(feathersAuthentication.authenticate({ type: 'local', email, password })).then().catch();
dispatch(feathersAuthentication.logout());

// Use Redux authentication, authorization packages to control React routes.
import { UserAuthWrapper } from 'redux-auth-wrapper';
const UserIsAuthenticated = UserAuthWrapper({
  // extract user data from state
  authSelector: (state) => state.auth.user, // THE REASON TO USE THIS REPO
  ...
});
const UserIsAdmin = UserAuthWrapper({
  authSelector: (state) => state.auth.user, // THE REASON TO USE THIS REPO
  ...
});

// React routing
<Provider store={store}>
  <Router history={history}>
    <Route path="/" component={AppWrapper}>
      <IndexRedirect to="/app" />
      <Route path="/app" component={UserIsAuthenticated(App)} />
      ...
      <Route path="/user/profilechange" component={UserIsAuthenticated(UserProfileChange)} />
      <Route path="/user/roleschange"
        component={UserIsAuthenticated(UserIsAdmin(UserRolesChange))}
      />
    </Route>
  </Router>
</Provider>
```

## <a name="motivation"></a> Motivation

- Feathers is a great real-time client-server framework.
- Redux is a great state container for the front-end.
- React is a great declarative UI.
- React-Router is a complete routing library for React by React.
- There are several packages
which handle authentication and authorization for redux and React-Router.

This repo let's everyone work together easily.

## <a name="installation"></a> Installation

Install [Nodejs](https://nodejs.org/en/).

Run `npm install feathers-reduxify-authentication --save` in your project folder.

You can then:

```javascript
// ES5
import feathersReduxifyAuthentication from 'feathers-reduxify-authentication';
// ES5
const feathersReduxifyAuthentication = require('feathers-reduxify-authentication');
```

`/src` on GitHub contains the ES6 source.

## <a name="apiReference"></a> API Reference

Each module is fully documented.

This package does some of the heavy lifting in
[feathers-starter-react-redux-login-roles](https://github.com/eddyystop/feathers-starter-react-redux-login-roles)
Review `client/feathers/index.js`, `client/reducers/index.js` and `client/index.js`.

## <a name="tests"></a> Tests

`npm test` to run tests.

`npm run cover` to run tests plus coverage.

## <a name="contribution"></a> Contributing

[Contribute to this repo.](./CONTRIBUTING.md)

[Guide to ideomatic contributing.](https://github.com/jonschlinkert/idiomatic-contributing)

## <a name="changeLog"></a> Change Log

[List of notable changes.](./CHANGELOG.md)

## <a name="license"></a> License

MIT. See LICENSE.