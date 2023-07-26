This fork adds an additional options argument to the authenticate function where you can set the returnUrl and realm options. This is very useful for multi-domain setups.

# Passport-OpenID

with [OpenID](http://openid.net/).

This module lets you authenticate using OpenID in your Node.js applications.  By
plugging into Passport, OpenID authentication can be easily and unobtrusively
integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

## Install

    $ npm install passport-openid

## Usage

#### Configure Strategy

The OpenID authentication strategy authenticates users using an OpenID
identifier.  The strategy requires a `validate` callback, which accepts this
identifier and calls `done` providing a user.  Additionally, options can be
supplied to specify a return URL and realm.

    passport.use(new OpenIDStrategy({
        returnURL: 'http://localhost:3000/auth/openid/return',
        realm: 'http://localhost:3000/'
      },
      function(identifier, done) {
        User.findByOpenID({ openId: identifier }, function (err, user) {
          return done(err, user);
        });
      }
    ));

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'openid'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

    app.post('/auth/openid',
      passport.authenticate('openid'));

    app.get('/auth/openid/return', 
      passport.authenticate('openid', { failureRedirect: '/login' }),
      function(req, res) {
        // Successful authentication, redirect home.
        res.redirect('/');
      });
      
#### Saving Associations

Associations between a relying party and an OpenID provider are used to verify
subsequent protocol messages and reduce round trips.  In order to take advantage
of this, an application must store these associations.  This can be done by
registering functions with `saveAssociation` and `loadAssociation`.

    strategy.saveAssociation(function(handle, provider, algorithm, secret, expiresIn, done) {
      // custom storage implementation
      saveAssoc(handle, provider, algorithm, secret, expiresIn, function(err) {
        if (err) { return done(err) }
        return done();
      });
    });

    strategy.loadAssociation(function(handle, done) {
      // custom retrieval implementation
      loadAssoc(handle, function(err, provider, algorithm, secret) {
        if (err) { return done(err) }
        return done(null, provider, algorithm, secret)
      });
    });

## Examples

For a complete, working example, refer to the

## Tests

    $ npm install --dev
    $ make test
