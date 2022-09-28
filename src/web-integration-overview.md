---
layout: base.njk
title: Web Integration Overview
---

Web integration in the NHS App is made possible by the NHS App Android and iOS native applications hosting a tailored webview along side navigation. The webview acts as a limited web browser, allowing users to navigate through a third party suppliers webpage, as if it is part of the actual app.

The aim of the web integration is to make the transition to and viewing off third party pages to be seamless for the user, therefore giving them an integrated experience. This allow the third party supplier to control the bulk of the user interface.

Conformance requirements are detailed in [the web integration specification document](https://digital.nhs.uk/binaries/content/assets/website-assets/services/nhs-app/suppliers/nhs-app-web-integration-specification-v1.1.pdf).

## Web View Handling Overview

The NHS app will send a custom user agent with all requests so that the supplier website knows it is being hosted within the NHS App browser. This will allow the supplier to:

* Hide site headers - the NHS App header and footer will be shown instead
* Show any NHS App specific functionality/components
* Use the NHS App JS API to invoke custom actions


### Navigation from NHS App to supplier

Navigation from the NHS App to web integration suppliers will be based on "Jump off points" located in set locations within logged in pages. SJR rules will decide whether or not individual jump off points for a given supplier are shown based on ODS code. The NHS App team will work directly with suppliers to map which jump off points will navigate for a given product.

## Process Flow

 <a href="/nhsapp-developer-documentation/images/WebIntegrationProcessFlow.png">
<img src="/nhsapp-developer-documentation/images/WebIntegrationProcessFlow.png" alt="Web Integration Process Flow" width="600"/>
</a>
