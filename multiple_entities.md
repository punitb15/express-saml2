# Multiple entities

For those applications with many clients, each client may have its own Identity Provider. Developers just need to configure multiple IdPs. Different configuration of SP is required for different IdP, therefore multiple SPs are allowed.

**Multiple IdPs - Single SP**

```javascript
// Define SP
var sp = saml.ServiceProvider('./metadata_sp.xml');
// Define multiple IdPs
var defaultIdP = saml.IdentityProvider('./metadata_idp_default.xml');
var oneLoginIdP = saml.IdentityProvider('./metadata_onelogin.xml');
// URL routing for SP-initiated SSO 
router.get('/spinitsso-post/:idp', function(req, res) {
    var toIdP;
    switch(req.params.idp || '') {
        case 'onelogin': {
            toIdP = oneLoginIdP;
            break;
        }
        default: {
            toIdP = idp;
            break;
        }
    }
    sp.sendLoginRequest(toIdP, 'post', function(request) {
        res.render('actions', request);
    });
});
```

Using the same SP configuration, it can apply to different IdPs. We've played a trick here, the initiated URL for SP-inititated SSO is controlled by a parameter. When user accesses `/spinitsso-post/onelogin`, the OneLogin IdP is used. When user accesses `/spinitsso-post/default`, the default IdP is then used for authentication.

**Multiple IdPs - Mutiple SPs**

Different IdPs may have different preference, so single SP configuration may not be suitable. For example, OneLogin IdP requires a request with signature but our default IdP does not. Here we come up with another solution which is very similar to the previous one.

```javascript
// Define a default SP
var spForDefault = saml.ServiceProvider('./metadata_sp.xml');
// Define SP for OneLogin with different metadta
var spForOnelogin = saml.ServiceProvider('./metadata_sp_for_oneLogin.xml');
// Define a default IdP
var defaultIdP = saml.IdentityProvider('./metadata_idp_default.xml');
// Define OneLogin IdP
var oneLoginIdP = saml.IdentityProvider('./metadata_onelogin.xml');
// URL routing for SP-initiated SSO 
router.get('/spinitsso-post/:idp', function(req, res) {
    var toIdP, fromSP;
    switch(req.params.idp || '') {
        case 'onelogin': {
            toIdP = oneLoginIdP;
            fromSP = spForOnelogin;
            break;
        }
        default: {
            toIdP = idp;
            fromSP = spForDefault;
            break;
        }
    }
    fromSP.sendLoginRequest(toIdP, 'post', function(request) {
        res.render('actions', request);
    });
});
```