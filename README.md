# github-oauth-prompt [![Build Status](https://secure.travis-ci.org/henrahmagix/github-oauth-prompt.png?branch=master)](http://travis-ci.org/henrahmagix/github-oauth-prompt) [![Coverage Status](https://coveralls.io/repos/henrahmagix/github-oauth-prompt/badge.png)](https://coveralls.io/r/henrahmagix/github-oauth-prompt)

[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/henrahmagix/github-oauth-prompt?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Easy creation of GitHub OAuth tokens.

Two-factor authentication is fully supported.

## Getting Started
Install the module with: `npm install github-oauth-prompt`

```javascript
var oauth = require('github-oauth-prompt');
oauth({name: 'my-token'}, function (err, token) {
    // Now you have a token.
});
```

## Documentation
_(Coming soon)_

See <a href="#examples">Examples</a>

## Examples
In all examples below, `callback` is a function accepting two parameters: error and response.
```js
function callback (err, res) {
    console.log('Token: ' + res);
}
```

### Example 1

```javascript
// Prompt for username, password, two-factor auth code if required,
// and return to the callback a GitHub token with basic scope.
oauth({
    name: 'moonrise-kingdom'
}, callback);
```

![moonrise-kingdom demo gif](http://i.imgur.com/f2VbL6m.gif)

### Example 2

```javascript
// Prompt for a token for read/write access to all repositories
// and organisations and write access to Gists.
// More scopes: https://developer.github.com/v3/oauth/#scopes
oauth({
    name: 'rushmore',
    scopes: ['repo', 'gist']
}, callback);
```

![rushmore demo gif](http://i.imgur.com/msW2ZYc.gif)

See that the token exists.

![rushmore token in list](http://i.imgur.com/9Of7p3Z.png)

When called again, the existing token is returned.

![rushmore demo existing gif](http://i.imgur.com/2xDlLpi.gif)

### Example 3

```javascript
// Set prompt messages.
oauth({
    name: 'the-life-aquatic',
    prompt: {
        username: 'Enter username:',
        password: 'Enter password:',
        code: 'Enter two-factor authorisation code'
    }
}, callback);
```

![the-life-aquatic demo gif](http://i.imgur.com/oj3kgwP.gif)

### Example 4

```javascript
// Use own prompt. Must deal with two-factor authentication.
// Ask for username and password and store in an object. For this example, and
// to avoid getting bogged down in callback-hell, we have them already available.
var auth = {
    username: 'Margot',
    password: 'RichieLovesMe'
};
// Setup a function to call oauth with a code.
function getToken (code) {
    // Get a token with a two-factor authentication code.
    var authOptions = {
        name: 'the-royal-tenenbaums'
        username: auth.username,
        password: auth.password,
    };
    if (code) {
        authOptions.code = code;
    }
    oauth(authOptions, function (err, token) {
        console.log(token);
    });
}
// Test to see if a code is required for a given username and password.
oauth.requiresCode(auth, function (err, hasTwoFactorAuth) {
    if (!hasTwoFactorAuth) {
        // No need for a code.
        getToken();
    } else {
        // You need to get a two-factor authentication code from
        // the user.
        myCodePrompt(function (err, code) {
            getToken(code);
        });
    }
});
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

```bash
# Requirements
npm install -g grunt-cli
# Dev
npm install && grunt
```

## Release History
- `v1.0.1` The Royal Tenenbaums: "Anybody interested in grabbing a couple of burgers and hittin' the cemetery?"
- `v1.0.0` **The Royal Tenenbaums**
- `v0.2.2` Rushmore: "What's the secret, Max?"
- `v0.2.1` Rushmore: "I saved Latin. What did you ever do?"
- `v0.2.0` **Rushmore**
- `v0.1.3` Bottle Rocket: "So, did you enjoy your first visit to the nut house?
- `v0.1.2` Bottle Rocket: "Tell Anthony I love him."
- `v0.1.1` Bottle Rocket: "Which part of Mexico are you from?"
- `v0.1.0` **Bottle Rocket**

## License
Copyright (c) 2014 Henry Blyth. Licensed under the MIT license.
