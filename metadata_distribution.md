# Metadata distribution

Metadata is used to exchange information between Identity Provider and Service Provider. Simply there are two major ways to exchange Metadata.

**1. Release in public **

Display the Metadata in a specific URL. Everyone has the URL can watch the Metadata. Therefore, the Metadata is distributed publicly. We provide an API to do it once you've configure your SP.

```javascript
var saml = require('express-saml2');
var sp = saml.ServiceProvider('./metadata_sp.xml');
router.get('/metadata',function(req, res, next) {
    res.header('Content-Type', 'text/xml').send(sp.getMetadata());
});
```

If you use our IdP solution, you can also release the Metadata same as above.

```javascript
var idp = saml.IdentityProvider('./metadata_idp.xml');
router.get('/metadata',function(req, res, next) {
    res.header('Content-Type', 'text/xml').send(idp.getMetadata());
});
```

**2. Export and distribute it privately**

Some properties may not want their Metadata to release publicly. If IdP/SP is configured using a setting object, we also provide an API to export the auto-generated Metadata.

```javascript
// Configure the entities without metadata
var sp = saml.ServiceProvider(spSetting);
var idp = saml.IdentityProvider(idpSetting);
// Export the auto-generated Metadata to a specific file
sp.exportMetadata('./distributed_sp_md.xml');
idp.exportMetadata('./distributed_idp_md.xml');
```

