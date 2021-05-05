---
layout: base.njk
title: Javascript API Specification
---

Any application embedded within the NHS App as part of a web integration has access to a limited number of methods to interact with the native Android or iOS Applications using an exposed Javascript interface.

To inline load the Javascript interface:

```javascript
<script type="text/javascript" src="<env-url>/js/v1/nhsapp.js"></script>
```

Where `env-url` will be determined by the environment being used. Unless testing  un-released functionality, the NHS App production environment can be used `https://www.nhsapp.service.nhs.uk/`.

Example Usage:

```javascript
nhsapp.navigation.goToHomepage()
```

## Reference
* [Tools](/nhsapp-developer-documentation/js-api-specification#tools)
  * [isOpenInNHSApp](/nhsapp-developer-documentation/js-api-specification#isOpenInNHSApp)
* [Navigation](/nhsapp-developer-documentation/js-api-specification#navigation)
  * [goToHomepage](/nhsapp-developer-documentation/js-api-specification#goToHomepage)
  * [goToPage](/nhsapp-developer-documentation/js-api-specification#goToPage)
* [Storage](/nhsapp-developer-documentation/js-api-specification#storage)
  * [addToCalendar](/nhsapp-developer-documentation/js-api-specification#addToCalendar)
  * [downloadFromBytes](/nhsapp-developer-documentation/js-api-specification#downloadFromBytes)

### Tools <a name="tools"></a>

#### isOpenInNHSApp Function <a name="isOpenInNHSApp"></a>

`nhsapp.tools.isOpenInNHSApp()`

##### Description

Returns true if the current page is open within the NHS App. Check is based on the current User Agent.

##### Status

Live

---

### Navigation <a name="navigation"></a>

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
| ADVICE                  | Advice Hub Page                                                                                                             |
| APPOINTMENTS            | Appointments Hub Page                                                                                                       |
| CORONAVIRUS_SERVICES    | Coronavirus Services Hub Page                                                                                               |
| HEALTH_RECORDS          | Health Record Hub Page                                                                                                      |
| HOME_PAGE               | NHS App Home Page                                                                                                           |
| MESSAGES                | Messages Hub Page                                                                                                           |
| PRESCRIPTIONS           | Prescriptions Hub Page                                                                                                      |


##### Example Usage

```
nhsapp.navigation.goToPage(nhsapp.navigation.AppPage.MESSAGES)
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

```
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

In draft - subject to change

---
