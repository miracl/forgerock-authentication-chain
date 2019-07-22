# MIRACL Trust Authentication Module

[MIRACL Trust](https://miracl.com) is a service that acts as password-free secured online identity provider utilizing patented elliptic curve cryptography.
MIRACL Trust can be configured as external Access Manager OIDC identity provider utilizing Authentication Chain modules. The step-by-step manual is outlined below.

## Integration manual
The steps of reference integration constructing are listed below.

* Deploy Access Manager 5.5 as described in the [ForgeRock manual](https://backstage.forgerock.com/docs/am/5.5/quick-start-guide).
* Create OIDC interface at [MIRACL Trust Management Portal](https://trust.miracl.cloud) as described in the [MIRACL Trust manual](https://devdocs.trust.miracl.cloud/register-create-new-app/). Remember its application client ID and secret.
* Go to AM realm dashboard. Open “Authentication - Modules”.
* Click “(+) Add Module”.

* Fill the form.

    + Name: ```“MIRACL"```

    + Type: ```“Social Auth OpenID" ```

* Click “Create”.

* Fill the form.

    + Social Provider: ```“MIRACL”```
    + Client Id: ```The OIDC application client ID from Management Portal```
    + Client Secret: ```The OIDC application client secret from Management Portal```
    + Authentication Endpoint URL: ```“https://api.mpin.io/authorize”```
    + Access Token Endpoint URL: ```“https://api.mpin.io/oidc/token”```
    + User Profile Service URL: ```“https://api.mpin.io/oidc/userinfo”```
    + Use Basic Auth: ```Off```

* Click “Save Changes”.

* Go to “Account Provisioning” tab.

* Fill the form.

    + Account Provider: ```“org.forgerock.openam.authentication.modules.common.mapping.DefaultAccountProvider”```
    + Account Mapper: ```“org.forgerock.openam.authentication.modules.common.mapping.JsonAttributeMapper|*|MIRACL-“```
    + Account Mapper Configuration: ```“id=iplanet-am-user-alias-list”```
    + Attribute Mapper: ```“org.forgerock.openam.authentication.modules.common.mapping.JsonAttributeMapper|iplanet-am-user-alias-list|MIRACL-“```
    + Attribute Mapper Configuration: ```“personal.last_name=sn id=uid email.email=mail personal.nickname=cn personal.first_name=givenName”```

* Click “Save Changes”.
* Go to “Authentication - Chains”.
* Click “(+) Add Chain”.

* Fill the form.

    + Name: ```“MIRACLAuthenticationService”```

* Click “Create”.
* Click “(+) Add a Module”.

* Fill the form.

    + Select Module: ```“MIRACL”```
    + Select Criteria: ```“Required”```

* Click “Ok”.

* Click “Save Changes”.

* Go to “Authentication - Settings”.

* Go to “User Profile” tab.

    + Select User Profile: ```“Ignored”```

* Click “Save Changes”.

* Go to “Services”.

* Click “Social Authentication Implementations”. 

* If there is no such entry, click (+) Add a service, and choose a service type “Social Authentication Implementations”.

* Fill the form “Display Names”.

    + Key: ```“MIRACL”```
    + Value: ```“MIRACL”```

* Click “(+) add”.

* Fill the form “Authentication Chains”.

    + Key: ```“MIRACL”```
    + Value: ```“MIRACLAuthenticationService”```

* Click “(+) add”.

* Fill the form “Enabled Implementations”.

    + Add ```“MIRACL” ```

* Click “Save Changes”.


## Test the integration

Go to some page that is protected via Access Manager.
* MIRACL identity provider should be available as a “HyperEye” icon at the bottom of the page among the other social identity providers.
![ScreenShot](./forgerock_login_with_MIRACL.png)


# Disclaimer
The sample integration setup described herein is provided on an "as is" basis, without warranty of any kind, to the fullest extent permitted by law. MIRACL does not warrant or guarantee the individual success developers may have when following the integration manual on their development platforms or in production configurations.
MIRACL does not warrant, guarantee or make any representations regarding the use, results of use, accuracy, timeliness or completeness of any data or information relating to the integration manual. MIRACL disclaims all warranties, expressed or implied, and in particular, disclaims all warranties of merchantability, and warranties related to the integration manual, or any service or software related thereto.
MIRACL shall not be liable for any direct, indirect or consequential damages or costs of any type arising out of any action taken by you or others related to the integration manual.
