---
layout: base.njk
title: Javascript API v2 Specification
---

Any application embedded within the NHS App as part of a web integration has access to a limited number of methods to interact with the native Android or iOS Applications using an exposed Javascript interface.

The NHS App JS API should be loaded inline rather than being bundled into the client application codebase. This is so that any changes to the implementation of the API do not require client applications to be recompiled and redeployed.

This specification replaces the version 1 specification, which is deprecated. Documentation for v1 remains [available for reference here](/js-api-specification).

To include the Javascript interface:

``` javascript
<script type="text/javascript" src="<env-url>/js/v2/nhsapp.js"></script>
```

Where `env-url` will be determined by the environment being used. Unless testing un-released functionality, the NHS App production environment can be used `https://www.nhsapp.service.nhs.uk/`.

Example Usage:

``` javascript
nhsapp.navigation.goToHomePage()
```

## Using newer functionality

The Javascript file may be cached by our client and CDN for up to 1 year. If you are updating your code to use newer functionality [(changelog)](/js-v2-api-specification/#changelog) from this library, you should append a query string to the end of the URL to invalidate any existing cache. For example, you could append the date like `?v=2025-07-20` when you start using new functionality.

---

## Changelog <a name="changelog"></a>

### 1 September 2025

* Added `PATIENT_INITIATED_FOLLOW_UP_REQUESTS` and `PATIENT_INITIATED_FOLLOW_UP_START` appPage options to `goToPage` method.

### 29 July 2025

* Added `ACCOUNT` appPage option to `goToPage` method.

### 31 January 2025

* Added `GO_BACK` appPage option to `goToPage` method. This allows navigation back to the last page the user visited in the NHS App before the web integration.

---

## Reference

* [Changelog](/js-v2-api-specification/#changelog)
* [Tools](/js-v2-api-specification/#tools)
  * [getAppPlatform](/js-v2-api-specification/#getAppPlatform)
  * [isOpenInNHSApp](/js-v2-api-specification/#isOpenInNHSApp)
* [Navigation](/js-v2-api-specification/#navigation)
  * [setBackAction](/js-v2-api-specification/#setBackAction)
  * [clearBackAction](/js-v2-api-specification/#clearBackAction)
  * [goToHomePage](/js-v2-api-specification/#goToHomePage)
  * [goToPage](/js-v2-api-specification/#goToPage)
  * [openBrowserOverlay](/js-v2-api-specification/#openBrowserOverlay)
  * [openExternalBrowser](/js-v2-api-specification/#openExternalBrowser)
* [Storage](/js-v2-api-specification/#storage)
  * [addToCalendar](/js-v2-api-specification/#addToCalendar)
  * [downloadFromBytes](/js-v2-api-specification/#downloadFromBytes)

---

### Tools <a name="tools"></a>

#### getAppPlatform Function <a name="getAppPlatform"></a>

`nhsapp.tools.getAppPlatform()`

##### Description

Returns a string indicating the platform on which the current page is being displayed. Check is based on the current User Agent.

The returned string will be one of the following:

- "android" - indicates that the page is being displayed within the native Android NHS App.
- "ios" - indicates that the page is being displayed within the native iOS NHS App.
- "none" - indicates that the page is not being displayed within the native NHS App (i.e. it is probably being displayed in a web browser).

##### Status

Live

---

#### isOpenInNHSApp Function <a name="isOpenInNHSApp"></a>

`nhsapp.tools.isOpenInNHSApp()`

##### Description

Returns true if the current page is open within the NHS App. Check is based on the current User Agent.

##### Status

Live

---

### Navigation <a name="navigation"></a>

#### setBackAction Function <a name="setBackAction"></a>

`nhsapp.navigation.setBackAction(backAction: function)`

##### Description

Set the action that will be executed when a user executes the native back functionality on their device such as pressing the back button on Android or swiping back on iOS.

##### Parameters

| Parameters              |                                                                                                                                                                |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| backAction              | **Type:** Function *Required*<br>The function to be excuted when the native back is pressed.            

##### Example Usage

``` javascript
let backAction = () => nhsapp.navigation.goToPage(nhsapp.navigation.AppPage.MESSAGES)
nhsapp.navigation.setBackAction(backAction)
```
##### Status

Live

---

#### clearBackAction Function <a name="clearBackAction"></a>

`nhsapp.navigation.clearBackAction()`

##### Description

Clear the action that will be executed when a user executes the native back functionality on their device such as pressing the back button on Android or swiping back on iOS.
        

##### Example Usage

``` javascript
nhsapp.navigation.clearBackAction()
```
##### Status

Live

---

#### goToHomePage Function <a name="goToHomePage"></a>

`nhsapp.navigation.goToHomePage()`

##### Description

Redirects the user to the NHS App homepage. This will exit the user’s current journey without any warning.	

##### Status

Live

---

#### goToPage Function <a name="goToPage"></a>

`nhsapp.navigation.goToPage(appPage: string enum)`

##### Description
Redirects the user to a page in the NHS App specified by the appPage string enumeration.
##### Parameters

| Parameters              |                                                                                                                                                                |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| appPage                 | **Type:** nhsapp.navigation.AppPage *Required*<br>The page to navigate to.                                                                                                     |

##### AppPage enumerations

| Value                                | Destination                                                                     |
|--------------------------------------|---------------------------------------------------------------------------------|
| `HOME_PAGE`                            | NHS App Home Page                                                               |
| `SERVICES`                             | Services Hub Page                                                               |
| `YOUR_HEALTH`                          | Your Health Hub Page                                                            |
| `MESSAGES`                             | Messages Hub Page                                                               |
| `UPLIFT`                               | P9 Uplift Page                                                                  |
| `ACCOUNT`                              | Account Hub Page                                                                |
| `PATIENT_INITIATED_FOLLOW_UP_REQUESTS` | Patient-initiated follow-up (PIFU) requests Page                                |
| `PATIENT_INITIATED_FOLLOW_UP_START`    | Request a patient-initiated follow-up (PIFU) Page                               |
| `GO_BACK`                              | Go back to last page the user visited in the NHS App before the web integration |


##### Example Usage

``` javascript
nhsapp.navigation.goToPage(nhsapp.navigation.AppPage.MESSAGES)
```

##### Status

Live

---

#### openBrowserOverlay Function <a name="openBrowserOverlay"></a>

`nhsapp.navigation.openBrowserOverlay(overlayUri: Uri)`

##### Description

Redirects the user to a specified page that will open in a browser overlay view.

##### Parameters

| Parameters              |                                                                                                                                                                |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| overlayUri              | **Type:** Uri *Required*<br>The page to open in the browser overlay.                                                                                                     |

##### Example Usage

``` javascript
nhsapp.navigation.openBrowserOverlay("https://www.nhs.uk/")
```

##### Status

Live

---

#### openExternalBrowser Function <a name="openExternalBrowser"></a>

`nhsapp.navigation.openExternalBrowser(browserUri: Uri)`

##### Description

Redirects the user to a specified page that will open in the device's default browser application.

##### Parameters

| Parameters              |                                                                                                                                                                |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| browserUri              | **Type:** Uri *Required*<br>The page to open in the browser.                                                                                                     |

##### Example Usage

``` javascript
nhsapp.navigation.openExternalBrowser("https://www.nhs.uk/")
```

##### Status

Live

---

### Storage <a name="storage"></a>

#### addEventToCalendar Function <a name="addToCalendar"></a>

```	javascript
nhsapp.storage.addEventToCalendar(
  subject: string,
  body: string,
  location: string,
  startTimeEpochInSeconds: integer,
  endTimeEpochInSeconds: integer,
)
```

##### Description

Add an event to the calendar on the user's device. The NHS App's native controls will alert the user of any issues adding the event to their calendar.

#### Parameters

| Parameters              |                                                                                                                                                                |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| subject                 | **Type:** string *Required*<br>The Subject line for the calendar appointment.                                                                                                            |
| body                    | **Type:** string *Required*<br>Plain text body for the appointment.                                                                                                                      |
| location                | **Type:** string *Required*<br>Plain text location for the appointment.                                                                                                                  |
| startTimeEpochInSeconds | **Type:** number *Required*<br>Start datetime for the appointment, set as a Epoch timestamp.                                                                                             |
| endTimeEpochInSeconds   | **Type:** number *Optional*<br>End datetime for the appointment, set as a Epoch timestamp. Must be greater or equal to `startTimeEpochInSeconds`.  Defaults to the same value as the start time (matching default behaviour of iOS and Android). |

##### Example Usage

``` javascript
nhsapp.storage.addEventToCalendar(
	"Hospital Appointment",
	"Please show up",
	"Dr Surgery, LS1",
	1592211600,
	1592215200
)
```

##### Status

Live

---

#### downloadFromBytes Function <a name="downloadFromBytes"></a>

``` javascript
nhsapp.downloadFromBytes(
  base64data: string,
  filename: string,
  mimeType: string
)
```

##### Description

Download a file to the user's device. The NHS App's native controls will prompt the user to select a location.

The mime type `application/vnd.apple.pkpass` is supported on the iOS platform and can be used to add a pass into the iOS wallet. The [getAppPlatform](/js-v2-api-specification/#getAppPlatform) api can be used to determine the current application platform.

#### Parameters

| Parameters              |                                                                                                                                                                |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| base64data              | **Type:** string *Required*<br>A base64 string representation of the file.                                                                                                            |
| filename                | **Type:** string *Required*<br>The name to download the file, including extension.                                                                                                                      |
| mimeType                | **Type:** string *Required*<br>The mime type of the file.                                                                                                                  |

##### Example Usage

``` javascript
var byteString = '....';
nhsapp.storage.downloadFromBytes(
  byteString,
  'results.pdf',
  'application/pdf'
);
```

##### Status

Live
