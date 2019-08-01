# MIRACL Trust Authentication Module

[MIRACL Trust® ID](https://miracl.com) is a 100% software solution providing true two-factor authentication using the latest [Zero Knowledge Proof (ZKP)](https://www.miracl.com/zero-knowledge) technology - no personal data is stored or transmitted. This eliminates the need for outdated security practices such as passwords, SMS Texts, push notifications and key-cards.

Usability is paramount, no additional user enrolment steps are required and users authenticate in one step. Without the requirement for hardware, mobiles or fiddly second steps, your users will love the simplicity and you will appreciate the reduction in support and maintenance overheads.

Provisioned as a PAYG service at a fraction of the cost of hardware solutions or SMS texts, MIRACL Trust® ID can be integrated into any OIDC compliant platform such as ForgeRock. Capable of scaling to millions of users overnight, a combination of usability, security and cost make it possible to deploy strong MFA to large B2C networks where it was previously impossible or impractical.
MIRACL Trust® ID makes a perfect addition to any large-scale ForgeRock installation.

[![Play Video](images/video.png)](https://www.youtube.com/watch?v=h-ccPbfWI7A)

Please visit [https://miracl.com](https://miracl.com) or email forgerock@miracl.com for details.

Create a MIRACL Trust® account and get 1,000 uses/month free forever - [Get Started](https://trust.miracl.cloud/get-started)


## Integration manual

The steps of reference integration constructing are listed below.

* Deploy Access Manager 5.5 as described in the [ForgeRock manual](https://backstage.forgerock.com/docs/am/5.5/quick-start-guide).

* Create OIDC interface at [MIRACL Trust Management Portal](https://trust.miracl.cloud) as described in the [MIRACL Trust manual](https://devdocs.trust.miracl.cloud/register-create-new-app/). Remember its application client ID and secret.

* Go to AM realm dashboard. Open "Authentication - Modules".

* Click "(+) Add Module".

* Fill the form.
    + Name: `"MIRACL"`
    + Type: `"Social Auth OpenID" `
* Click "Create".

* Fill the form.
    + Social Provider: `"MIRACL"`
    + Client Id: `The OIDC application client ID from Management Portal`
    + Client Secret: `The OIDC application client secret from Management Portal`
    + Authentication Endpoint URL: `"https://api.mpin.io/authorize"`
    + Access Token Endpoint URL: `"https://api.mpin.io/oidc/token"`
    + User Profile Service URL: `"https://api.mpin.io/oidc/userinfo"`
    + Use Basic Auth: `Off`
    + Subject Property: `"uid"`
    + Proxy URL: Leave the default value which should be something like this `"http://xxx.xxx/openam/oauth2c/OAuthProxy.jsp"` where `xxx.xxx` is your domain
      + Go to [MIRACL Trust Management Portal](https://trust.miracl.cloud)
      + Set Redirect URL to: `Proxy URL`
    + Token Issuer: `"https://api.mpin.io"`
* Click "Save Changes".

* Go to "OpenID Connect" tab.
  + OpenID Connect validation configuration type: `jwk_url`
  + OpenID Connect validation configuration value: `"https://api.mpin.io/oidc/certs"`
* Click "Save Changes".

* Go to "Account Provisioning" tab.
* Fill the form.
    + Account Provider: `"org.forgerock.openam.authentication.modules.common.mapping.DefaultAccountProvider"`
    + Account Mapper: `"org.forgerock.openam.authentication.modules.common.mapping.JsonAttributeMapper|*|MIRACL-"`
    + Account Mapper Configuration: `"id=iplanet-am-user-alias-list"`
    + Attribute Mapper: `"org.forgerock.openam.authentication.modules.common.mapping.JsonAttributeMapper|iplanet-am-user-alias-list|MIRACL-"`
    + Attribute Mapper Configuration: `"personal.last_name=sn id=uid email.email=mail personal.nickname=cn personal.first_name=givenName"`
* Click "Save Changes".

* Go to "Authentication - Chains".
* Click "(+) Add Chain".
* Fill the form.
    + Name: `"MIRACLAuthenticationService"`
* Click "Create".
* Click "(+) Add a Module".
* Fill the form.
    + Select Module: `"MIRACL"`
    + Select Criteria: `"Required"`
* Click "Ok".
* Click "Save Changes".

* Go to "Authentication - Settings".
* Go to "User Profile" tab.
    + Select User Profile: `"Ignored"`
* Click "Save Changes".

* Go to "Services".
* Click "Social Authentication Implementations".
* If there is no such entry, click (+) Add a service, and choose a service type "Social Authentication Implementations".

* Fill the form "Display Names".
    + Key: `"MIRACL"`
    + Value: `"MIRACL"`
* Click "(+) add".

* Fill the form "Authentication Chains".
    + Key: `"MIRACL"`
    + Value: `"MIRACLAuthenticationService"`
* Click "(+) add".

* Fill the form "Enabled Implementations".
    + Add `"MIRACL"`

* Click "Save Changes".


## Test the integration

* Go to AM realm dashboard. Open "Authentication - Settings".
* Go to "Core" tab.
    + Select Organization Authentication Configuration: `"MIRACLAuthenticationService"`
* Click "Save Changes".

* Once you change that, the default log in to AM will be `"MIRACLAuthenticationService"` tree. If you want to go back to the administrator window to make changes to the configuration go to AM URL by appending `"/console"` (for example, `"http://openam.partner.com:8080/openam/console"`). This will use the administrator service to log in to AM (which should be the `"ldapService"`).

* Logout from Access Manager and try to login again.
* AM should automatically redirect you to the MIRACL identity provider.


# Disclaimer

The sample integration setup described herein is provided on an "as is" basis, without warranty of any kind, to the fullest extent permitted by law. MIRACL does not warrant or guarantee the individual success developers may have when following the integration manual on their development platforms or in production configurations.
MIRACL does not warrant, guarantee or make any representations regarding the use, results of use, accuracy, timeliness or completeness of any data or information relating to the integration manual. MIRACL disclaims all warranties, expressed or implied, and in particular, disclaims all warranties of merchantability, and warranties related to the integration manual, or any service or software related thereto.
MIRACL shall not be liable for any direct, indirect or consequential damages or costs of any type arising out of any action taken by you or others related to the integration manual.
