# passport-accountkit-token

[Passport](http://passportjs.org/) strategy for authenticating with [Facebook Account Kit](http://www.facebook.com/)
access tokens using the OAuth 2.0 API.

This module lets you authenticate using Facebook Account Kit in your Node.js applications.
By plugging into Passport, Facebook authentication can be easily and
unobtrusively integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

## Installation

    $ npm install passport-accountkit-token

## Usage

### Configure Strategy

The Facebook authentication strategy authenticates users using a Facebook
account and OAuth 2.0 tokens.  The strategy requires a `verify` callback, which
accepts these credentials and calls `done` providing a user, as well as
`options` specifying a app ID and app secret.

```js
var AccountKitTokenStrategy = require('passport-facebook-token');

passport.use(new AccountKitTokenStrategy({
    clientID: FACEBOOK_APP_ID,
    clientSecret: FACEBOOK_APP_SECRET
  }, function(accessToken, refreshToken, profile, done) {
    User.findOrCreate({facebookId: profile.id}, function (error, user) {
      return done(error, user);
    });
  }
));
```

### Authenticate Requests

Use `passport.authenticate()`, specifying the `'accountkit-token'` strategy, to authenticate requests.

```js
app.post('/auth/accounkit/token',
  passport.authenticate('accountkit-token'),
  function (req, res) {
    // do something with req.user
    res.send(req.user? 200 : 401);
  }
);
```

Or using Sails framework:

```javascript
// api/controllers/AuthController.js
module.exports = {
  facebook: function(req, res) {
    passport.authenticate('accountkit-token', function(error, user, info) {
      // do stuff with user
      res.ok();
    })(req, res);
  }
};
```

### Client Requests

Clients can send requests to routes that use passport-facebook-token authentication using query parms, body, or HTTP headers. Clients will need to transmit the `access_token`
and optionally the `refresh_token` that are received from facebook after login.

#### Sending access_token as a Query parameter

```
GET /auth/facebook/token?access_token=<TOKEN_HERE>
```

#### Sending access token as an HTTP header

Clients can choose to send the access token using the Oauth2 Bearer token (RFC 6750) compliant format

```
GET /resource HTTP/1.1
Host: server.example.com
Authorization: Bearer base64_access_token_string
```

optionally a client can send via a custom (default access_token) header

```
GET /resource HTTP/1.1
Host: server.example.com
access_token: base64_access_token_string
```

#### Sending access token as an HTTP body

Clients can transmit the access token via the body

```
GET /resource HTTP/1.1
Host: server.example.com

access_token=base64_access_token_string
```

## License

The MIT License (MIT)

Copyright (c) 2015 Nicholas Penree

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
