# Service Provider

The API will be updated above version 2.0 to reduce the ambiguity. In basic tutorial, we only construct the service provider with metadata. We don't suppose to sign the SAML Request. If you need to sign your SAML Request, you may go to advanced section for more detail.

Let's get started to get the entry point:
```javascript
var saml = require('express-saml2');
```

And you should have prepared the metadata of service provider:
```xml
<EntityDescriptor
 xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata"
 xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
 xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
 entityID="https://sp.example.org/metadata">
    <SPSSODescriptor WantAssertionsSigned="true" protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
        <KeyDescriptor use="signing">
            <KeyInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
                <X509Data>
                    <X509Certificate>MIID...</X509Certificate>
                </X509Data>
            </KeyInfo>
        </KeyDescriptor>
        <NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress</NameIDFormat>
        <AssertionConsumerService isDefault="true" index="0" Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="https://sp.example.org/acs"/>
    </SPSSODescriptor>
</EntityDescriptor>
```
Import the metadata and work is done:
```javascript
var sp = saml.ServiceProvider('./metadata/sp.xml');
```