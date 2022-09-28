---
layout: base.njk
title: Web Integration Step by Step
---

* [Hiding Headers](/nhsapp-developer-documentation/web-integration-step-by-step#hideheaders)
* [Single Sign On](/nhsapp-developer-documentation/web-integration-step-by-step/#sso)
* [Site Links](/nhsapp-developer-documentation/web-integration-step-by-step/#sitelinks)
* [Limitations](/nhsapp-developer-documentation/web-integration-step-by-step/#limitations)

## Hiding Headers<a name="hideheaders"></a>

In order to fit in with the styling of the NHS App - suppliers are required to hide the header of their service - which will be superseded by the NHS App native header.

The NHS App will identify itself by adding a custom string in the format `nhsapp-android/<nhsapp-version>` or `nhsapp-ios/<nhsapp-version>` to the end of the user agent header when making requests (where nhsapp-version is the current version of the native NHS App, e.g. 1.29.0).

### Examples

| Device  | App Version | Custom User Agent                                                                                                                               |
|---------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| Pixel 2 | 1.26.2      | Mozilla/5.0 (Linux; Android 10; Pixel 2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/77.0.3865.116 Mobile Safari/537.36 nhsapp-android/1.26.2 |
| Phone   | 1.25.0      | Mozilla/5.0 (iPhone; CPU iPhone OS 6_1_2 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Mobile/10B146 nhsapp-ios/1.25.0                  |

We recommend using the [isOpenInNHSApp function in the NHS App JS API](/nhsapp-developer-documentation/js-api-specification/#isOpenInNHSApp) to determine if the page is open in the NHS App which uses the user agent for detection.

## NHS login Single Sign On <a name="sso"></a>

Logged in users of the application will be passed to third party services using [NHS login Single Sign On flow](https://nhsconnect.github.io/nhslogin/single-sign-on/). 

Single Sign on will be achieved by passing the NHS App JWT token via a query parameter. The NHS App will allow a supplier to provide one or more url paths to be linked to from the jump off points.

Suppliers will provide the following details to the NHS App prior to an integration:

* Base Url (for each environment stage)
* Journey Path(s) 

The NHS App will then redirect to the supplier using the following url format 

`<baseurl>/<journey-path>?assertedLoginIdentity=<sso-jwt>`


E.g. `https://supplier1.com/hospital-appointments?assertedLoginIdentity=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

The `assertedLoginIdentity` query parameter will be added as the last parameter, so it is possible for the journey path to include query parameters.

### NHS login - Authorize call
The NHS requires suppliers to pass `prompt=none` to the NHS login `/authorize` endpoint. This will ensure that the user is shown an error page if there is an SSO issue. Suppliers must be able to display a suitable error to the user if an OIDC error code is returned from NHS login. In most circumstances, this error screen should prompt the user to return back to the NHS App to re-authenticate.

**Note:** the [NHS login SSO documentation](https://nhsconnect.github.io/nhslogin/single-sign-on/) outlines the need for the Asserted Login Identity parameter to be passed in a different format than the `assertedLoginIdentity` query parameter (Camel case) supplied by the NHS App as outlined above. NHS login require this to be converted to Snake case: `asserted_login_identity`.

### NHS login - Handle "I do not want to use my NHS login"
NHS login require suppliers to handle the return from this page where users are asked to agree to sharing their login details with the supplier.

If the user selects "I do not want to use my NHS login", NHS login will return to the suppliers `auth-return` endpoint with `error=access_denied` and `error_description=ConsentNotGiven`.

The supplier should handle this and present a suitable message, with a link to the NHS App home page.

## Site Links <a name="sitelinks"></a>

It is possible to allow users to open links that suppliers send out (e.g. via email or sms) within the NHS App. This requires changes on both the NHS App and the supplier's application. Suppliers should work with the NHS App onboarding team to co-ordinate the changes.

### Android

Site links are achieved on Android using [Android App Links](https://developer.android.com/training/app-links/verify-site-associations). This will require the supplier to host a *Asset Links* file on `https://<domain-name>/.well-known/assetlinks.json` with the contents:

```json
[
    {
        "relation": [
            "delegate_permission/common.handle_all_urls"
        ],
        "target": {
            "namespace": "android_app",
            "package_name": <package-name>,
            "sha256_cert_fingerprints": [
                <cert-fingerprint>
            ]
        }
    }
]
```

where `package-name` and `cert-fingerprint` vary per NHS App environment. These values will be provided by the NHS App team during integration.

### iOS

Site links are achieved on iOS using [Associated Domains](https://developer.apple.com/documentation/safariservices/supporting_associated_domains). This will require the supplier to host a *Site Association* file on `https://<domain-name>/.well-known/apple-app-site-association` with the contents:

```json
{
  "applinks": {
    "apps": [],
    "details": [
      {
        "appID": "<app-id>",
        "paths": [ <paths>]
      }
    ]
  }
}
```

where app-id will vary per NHS App environment and will be provided by the NHS App team during integration. Paths will contain a list of url paths that the supplier would like to open on the NHS App.

## Limitations <a name="limitations"></a>

Although, the NHS App iOS and Android apps act largely as web browsers - there are some web functions that are not available. In some cases these will be addressed by the [NHS App JS API](/nhsapp-developer-documentation/js-api-specification).

| Action             | Notes/Workarounds |
|--------------------|-------------------|
| File Download      | Conventional file download does not work in web integrations due to webview limitations. However, there is a function in the NHS App JS Api to allow users to [download a file from bytes](/nhsapp-developer-documentation/js-api-specification#downloadFromBytes).|
| Browser Print      | There is no plan to implement this in the NHS App as this functionality could confuse users as they are viewing the page in a native app.|
