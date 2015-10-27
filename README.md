[express-saml2](https://github.com/tngan/express-saml2)
=======
express-saml2 is a high-level API library for Single Sign On with SAML 2.0

This module provides a library for scaling Single Sign On implementation. Developers can easily configure the entities by importing the metadata. 

We provide a simple interface that's highly configurable.


### Installation
To install the stable version
```bash
$ npm install express-saml2
```

### Use cases

+ IdP-initiated Single Sign On
+ IdP-initiated Single Log-out
+ SP-initiated Single Sign On
+ SP-initiated Single Log-out (in development)

Simple solution of Identity Provider is provided in this module for test and educational use. Work with other 3rd party Identity Provider is also supported.

### Glimpse of code

```javascript
var saml = require('express-saml2');
// Configure a service provider
var sp = saml.ServiceProvider('./metadata_sp.xml');
// Configure the corresponding identity provider
var idp = saml.IdentityProvider('./metadata_idp.xml');
// Parse when receive a SAML Response from IdP
router.post('/acs', function (req, res, next) {
    sp.parseLoginResponse(idp, 'post', req, function (parseResult) {
        // Write your own validation and render function here
    });
});
```
Our default validation is to validate signature and the issuer name of Identity Provider. The code looks intuitive and easy for reading. More use cases are provided in this documentation to fit in the real world application.

### License

MIT
