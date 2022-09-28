---
layout: base.njk
title: Javascript API Specification
---

Any application embedded within the NHS App as part of a web integration has access to a limited number of methods to interact with the native Android or iOS Applications using an exposed Javascript interface.

The NHS App JS API must be loaded inline rather than being bundled into the client application codebase. This is so that any changes to the implementation of the API do not require client applications to be recompiled and redeployed.

To include the Javascript interface:

```javascript
<script type="text/javascript" src="<env-url>/js/v1/nhsapp.js"></script>
```

Where `env-url` will be determined by the environment being used. Unless testing un-released functionality, the NHS App production environment can be used `https://www.nhsapp.service.nhs.uk/`.

Example Usage:

```javascript
nhsapp.navigation.goToHomepage()
```

## Reference
* [Tools](/nhsapp-developer-documentation/js-api-specification#tools)
  * [getAppPlatform](/nhsapp-developer-documentation/js-api-specification#getAppPlatform)
  * [isOpenInNHSApp](/nhsapp-developer-documentation/js-api-specification#isOpenInNHSApp)
* [Navigation](/nhsapp-developer-documentation/js-api-specification#navigation)
  * [setBackAction](/nhsapp-developer-documentation/js-api-specification#setBackAction)
  * [clearBackAction](/nhsapp-developer-documentation/js-api-specification#clearBackAction)
  * [goToHomepage](/nhsapp-developer-documentation/js-api-specification#goToHomepage)
  * [goToPage](/nhsapp-developer-documentation/js-api-specification#goToPage)
  * [openBrowserOverlay](/nhsapp-developer-documentation/js-api-specification#openBrowserOverlay)
  * [openExternalBrowser](/nhsapp-developer-documentation/js-api-specification#openExternalBrowser)
* [Storage](/nhsapp-developer-documentation/js-api-specification#storage)
  * [addToCalendar](/nhsapp-developer-documentation/js-api-specification#addToCalendar)
  * [downloadFromBytes](/nhsapp-developer-documentation/js-api-specification#downloadFromBytes)

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
| backAction                 | **Type:** Function *Required*<br>The function to be excuted when the native back is pressed.            

##### Example Usage

```js
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

```js
nhsapp.navigation.clearBackAction()
```
##### Status

Live

---

#### goToHomepage Function <a name="goToHomepage"></a>

`nhsapp.navigation.goToHomepage()`

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

| Values              |                                                                                                                                                                |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HOME_PAGE               | NHS App Home Page                                                                                                           |
| ADVICE                  | Advice Hub Page                                                                                                             |
| APPOINTMENTS            | Appointments Hub Page                                                                                                       |
| HEALTH_RECORDS          | Health Record Hub Page                                                                                                      |
| MESSAGES                | Messages Hub Page                                                                                                           |
| PRESCRIPTIONS           | Prescriptions Hub Page                                                                                                      |
| UPLIFT                  | P9 Uplift Page                                                                                                              |


##### Example Usage

```js
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
| overlayUri                 | **Type:** Uri *Required*<br>The page to open in the browser overlay.                                                                                                     |

##### Example Usage

```js
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

```js
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

```js
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

```	javascript
nhsapp.downloadFromBytes(
  base64data: string,
  filename: string,
  mimeType: string
)
```

##### Description

Download a file to the user's device. The NHS App's native controls will prompt the user to select a location.

The mime type `application/vnd.apple.pkpass` is supported on the iOS platform and can be used to add a pass into the iOS wallet. The [getAppPlatform](/nhsapp-developer-documentation/js-api-specification#getAppPlatform) api can be used to determine the current application platform.

#### Parameters

| Parameters              |                                                                                                                                                                |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| base64data                 | **Type:** string *Required*<br>A base64 string representation of the file.                                                                                                            |
| filename                    | **Type:** string *Required*<br>The name to download the file, including extension.                                                                                                                      |
| mimeType                | **Type:** string *Required*<br>The mime type of the file.                                                                                                                  |

##### Example Usage

```js
var byteString = '....';
nhsapp.storage.downloadFromBytes(
  byteString,
  'results.pdf',
  'application/pdf'
);
```

##### Status

Live

---
