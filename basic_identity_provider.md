# Identity Provider

This module provides API of Identity Provider for internal test purpose. Even though the API is released in open source, we still suggest developers to use other 3rd party IdP. No matter you use our IdP solution or not, it's still needed to do configuration in order to apply its preference.

Let's get started with entry point:
```javascript
var saml = require('express-saml2');
```

And the following metadata is provided by OneLogin:
```xml
<EntityDescriptor xmlns="urn:oasis:names:tc:SAML:2.0:metadata" entityID="https://app.onelogin.com/saml/metadata/486670">
  <IDPSSODescriptor xmlns:ds="http://www.w3.org/2000/09/xmldsig#" protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
    <KeyDescriptor use="signing">
      <ds:KeyInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        <ds:X509Data>
          <ds:X509Certificate>MIIEF...</ds:X509Certificate>
        </ds:X509Data>
      </ds:KeyInfo>
    </KeyDescriptor>
    <NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress</NameIDFormat>
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://esaml2.onelogin.com/trust/saml2/http-post/sso/486670"/>
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="https://esaml2.onelogin.com/trust/saml2/http-post/sso/486670"/>
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:SOAP" Location="https://esaml2.onelogin.com/trust/saml2/soap/sso/486670"/>
  </IDPSSODescriptor>
  <ContactPerson contactType="technical">
    <SurName>Support</SurName>
    <EmailAddress>support@onelogin.com</EmailAddress>
  </ContactPerson>
</EntityDescriptor
```
Import the above metadata and get the work done:
```javascript
var idp = saml.IdentityProvider('./metadata/onelogin_metadata_486670.xml');
```