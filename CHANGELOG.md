# Change Log

## 10.2.2 (2024-12-19)

### Patch Changes

- Introduced human-friendly timestamps in the default logger to improve traceability and debugging.
- Resolved an issue where the SDK could crash in certain scenarios when an error occurred during a request.
- Improved the consistency of logs for requests by updating similar messages and ensuring accurate display of both responses and errors.
- Implemented logging for EventSource status updates (connection, disconnection, and errors).
- Addressed a browser-related issue where, if consent was required or no consent was provided (no targeting delivery rules), newly generated visitor codes were stored in cookies after page reloads.
- Updated dependencies
  - @kameleoon/javascript-sdk@4.2.2

## 10.2.1 (2024-12-10)

### Patch Changes

- Replaced the nullish coalescing assignment operator (??=) with a more widely supported approach to ensure compatibility with older ES versions.
- Updated dependencies
  - @kameleoon/javascript-sdk@4.2.1

## 10.2.0 (2024-12-09)

### Features

- Added support for **simulated** variations.
- Added the [`setForcedVariation()`][setForcedVariation] method. This method allows explicitly setting a forced variation for a visitor, which will be applied during experiment evaluation.

### Patch Changes

- Resolved an issue where the validation of [cookie domains][cookieDomain] for `localhost` resulted in incorrect failures. The SDK now accepts the provided domain without modification if it is deemed invalid and logs an [error][logLevels] to notify you of any issues with the specified domain.
- Updated dependencies
  - @kameleoon/javascript-sdk@4.2.0

[setForcedVariation]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#setforcedvariation
[cookieDomain]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#configuration-parameters
[logLevels]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#log-levels

## 10.1.3 (2024-12-06)

### Patch Changes

- Fixed the issue with `off` variation not being added to `getEngineTrackingCode` result
- Updated dependencies
  - @kameleoon/javascript-sdk@4.1.3

## 10.1.2 (2024-12-05)

### Patch Changes

- Resolved an issue that caused server slowdowns when handling large numbers of unique visitors.
- Resolved an issue where setting the [`DEBUG`][loglevels] log level could cause unexpected crashes.
- Updated dependencies
  - @kameleoon/javascript-sdk@4.1.2

[loglevels]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#log-levels

## 10.1.1

### Patch Changes

- Fixed the issue where Real-Time Update server failures could lead to SDK configuration update issues. If the Real-Time Update server fails SDK will now temporarily switch to polling mode until the Real-Time Update server is available again.
- Added an optional `onError` handler for [`IExternalEventSource`][iextrnaleventsource] interface. If the `onError` handler is not provided the SDK would not be able to handle Real-Time Update server failures. Note: no action is required when using default Kameleoon event source solutions.
- Updated dependencies
  - @kameleoon/javascript-sdk@4.1.1

[iextrnaleventsource]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk/#eventsource

## 10.1.0 (2024-11-14)

### Features

- Enhanced top-level domain validation within the SDK. The implementation now includes automatic trimming of extraneous symbols.
- Enhanced [`logging`][logging]:
  - Introduced [`log levels`][loglevels]:
    - `None`
    - `Error`
    - `Warning`
    - `Info`
    - `Debug`
  - Added support for [`custom logger`][customlogger] implementations.
- Introduced new evaluation methods for clarity and improved efficiency when working with the SDK:
  - [`getVariation`][getVariation]
  - [`getVariations`][getVariations]
- Introduced methods replace the deprecated ones:
  - [`getFeatureFlags`][getFeatureFlags]
  - [`getVisitorFeatureFlags`][getVisitorFeatureFlags]
  - [`getActiveFeatureFlags`][getActiveFeatureFlags]
  - [`getFeatureFlagVariationKey`][getFeatureFlagVariationKey]
  - [`getFeatureFlagVariable`][getFeatureFlagVariable]
  - [`getFeatureFlagVariables`][getFeatureFlagVariables]
- A new version of the [`isFeatureFlagActive`][isFeatureFlagActive] method now has a new overload, which contains an optional `track` parameter for managing the assigned variation tracking (default: `true`).

### Patch Changes

- [`UniqueIdentifier`][uniqueidentifier] data is now correctly exported from the SDK
- Updated dependencies
  - @kameleoon/javascript-sdk@4.1.0

[getVariation]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getvariation
[getVariations]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getvariations
[getFeatureFlags]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getfeatureflags
[getVisitorFeatureFlags]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getvisitorfeatureflags
[getActiveFeatureFlags]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getactivefeatureflags
[getFeatureFlagVariationKey]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getfeatureflagvariationkey
[getFeatureFlagVariable]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getfeatureflagvariable
[getFeatureFlagVariables]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getfeatureflagvariables
[isFeatureFlagActive]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#isfeatureflagactive
[logging]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#logging
[loglevels]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#log-levels
[customlogger]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#custom-handling-of-logs

## 10.0.2 (2024-11-08)

### Patch Changes

- Fixed build issue resulting in potential issues when using any targeting condition with `Exclude` option
- Updated dependencies
  - @kameleoon/javascript-sdk@4.0.2

## 10.0.1 (2024-11-05)

### Patch Changes

- Fixed an issue with the [`Page URL`][Targeting Conditions] and [`Page Title`][Targeting Conditions] targeting conditions, where the condition evaluated all previously visited URLs in the session instead of only the current URL, corresponding to the latest added `PageView`
  NOTE: This change may impact your existing targeting. Please review your targeting conditions to ensure accuracy.
- Updated dependencies
  - @kameleoon/javascript-sdk@4.0.1

[Targeting Conditions]: https://developers.kameleoon.com/feature-management-and-experimentation/using-visit-history-in-feature-flags-and-experiments#benefits-of-calling-getremotevisitordata

## 10.0.0 (2024-10-03)

### Breaking Changes

- `isUniqueIdentifier` parameter has been removed from methods [`flush`][flush], [`getRemoteVisitorData`][getremotevisitordata] and [`trackConversion`][trackconversion]
- Method [`initialize`][initialize] no more accepts `useCache` boolean parameter. Using storage cached client configuration on failed configuration request is now default behavior.
- Previously deprecated method [`onConfigurationUpdate`][onconfigurationupdate] has been removed from SDKs. Use [`onEvent`][onevent] method with `EventType.ConfigurationUpdate` to achieve the same effect.
- Previously deprecated field [`domain`][domain] of `SDKConfiguration` has been removed from SDKs. Use [`cookieDomain`][domain] field of `SDKConfiguration` instead.
- Parameter `text` of `KameleoonResponseType` used in custom [`requester`][requester] implementation is now mandatory, for the vast majority of implementations like `fetch`, `axios` or `node-fetch` it won't require any changes.

### Features

- `FeatureVariableType` returned from the methods obtaining feature flag variables can now have two new `VariableType`s - `VariableType.JS` containing a `string` of JavaScript code and `VariableType.CSS` containing a string with CSS code
- Added new [`UniqueIdentifier`][uniqueidentifier] data to be used instead of removed `isUniqueIdentifier` parameters in some methods
- Added new [`SDKConfiguration`][sdkconfiguration] parameter `trackingInterval` to set the interval between SDK tracking network requests in _milliseconds_, default value is `1000`, which is also maximum interval, minimum value is `100`

### Patch Changes

- [`getEngineTrackingCode`][getenginetrackingcode] method now correctly sets `triggerExperiment` parameter based on variable types
- Updated dependencies
  - @kameleoon/javascript-sdk@4.0.0

[uniqueidentifier]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#uniqueidentifier
[sdkconfiguration]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#arguments
[flush]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#flush
[getremotevisitordata]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getremotevisitordata
[trackconversion]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#trackconversion
[getenginetrackingcode]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getenginetrackingcode
[onconfigurationupdate]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk/#onconfigurationupdate
[onevent]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk/#onevent
[domain]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#configuration-parameters
[requester]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk/#requester
[initialize]: https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#initialize

## 9.5.4 (2024-08-30)

### Patch Changes

- Fixed the issue - some feature experiment conditions could resolve to incorrect values on interval configuration update
- Updated dependencies
  - @kameleoon/javascript-sdk@3.4.7

## 9.5.3 (2024-08-15)

### Patch Changes

- Fixed the issue with mapping identifier Custom Data not being set correctly after browser tab reload
- Updated dependencies
  - @kameleoon/javascript-sdk@3.4.6

## 9.5.2 (2024-08-08)

### Patch Changes

- Fixed an issue that caused duplicate entries in feature flag results for both anonymous and authorized/identified visitors during data reconciliation. This problem occurred when custom data of type mapping ID was not consistently sent for all sessions.
- Updated dependencies
  - @kameleoon/javascript-sdk@3.4.5

## 9.5.1 (2024-07-19)

### Patch Changes

- Fix previous release, which didn't contain all the described changes
- Updated dependencies
  - @kameleoon/javascript-sdk@3.4.4

## 9.5.0 (2024-07-19)

### Features

- Added new external dependency [`Pseudo Random Number Generator`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#pseudo-random-number-generator)

### Patch Changes

- Fixed the issue where `useCache` parameter of [`initialize`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#initialize) wasn't displayed in the method parameters type
- Updated dependencies
  - @kameleoon/javascript-sdk@3.4.3

## 9.4.2 (2024-07-16)

### Patch Changes

- [`getVisitorCode`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getvisitorcode) now sets the visitor code to cookie correctly, when the visitor code was initially retrieved from it
- Updated dependencies
  - @kameleoon/javascript-sdk@3.4.2

## 9.4.1 (2024-07-12)

### Patch Changes

- `ClientConfiguration` and `RemoteData` Kameleoon Exceptions are now more informative
- [`getEngineTrackingCode`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk/#getenginetrackingcode) output code is now correctly overrides experiment variation assigned by JS Script
- Updated dependencies
  - @kameleoon/javascript-sdk@3.4.1

## 9.4.0 (2024-06-21)

### Features

- Added new [`networkDomain`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#1-create-kameleoon-client) parameter in `SDKConfigurationType` for configuring custom network domain for all outgoing requests.
- [`SDKConfigurationType`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#1-create-kameleoon-client) parameter `domain` was deprecated and will be removed in the next major version. Instead new `cookieDomain` is available.
- Consent value can now be read automatically from the JS Script if the visitor code is set in cookie.
- Add new optional [External Dependency](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#external-dependencies) - [Requester](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#requester) for implementing custom network request handler
- Added new `KameleoonUtils` methods:
  - [`getCookieValue`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getcookievalue) for extracting cookie value from cookie string
  - [`simulateSuccessRequest`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#simulatesuccessrequest) for mocking successful network requests when implementing custom `Requester`

### Patch Changes

- Updated dependencies
  - @kameleoon/javascript-sdk@3.4.0

## 9.3.0 (2024-05-24)

### Features

- Added new [`onEvent`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk/#onevent) method retuned from `useInitialize` hook to handle SDK events with a callback.
- Method [`onConfigurationUpdate`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk/#onConfigurationUpdate) is now marked as `deprecated` and will be removed in the next major release. Use [`onEvent(EventType.ConfigurationUpdate, callback)`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/js-sdk/#onevent) instead.
- Added support of range match type for numeric Custom Data values.

### Patch Changes

- Updated dependencies
  - @kameleoon/javascript-sdk@3.3.0

## 9.2.0 (2024-05-07)

### Features

- New method [`getActiveFeatureFlags`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk/#getactivefeatureflags) obtained with `useFeature` hook, which retrieves feature flags that are active for visitor with detailed variation and variables information.

### Patch Changes

- Updated dependencies
  - @kameleoon/javascript-sdk@3.2.0

## 9.1.0 (2024-04-19)

### Features

- New [Likelihood to Convert](https://developers.kameleoon.com/feature-management-and-experimentation/using-visit-history-in-feature-flags-and-experiments/#predefined-targeting-conditions) targeting condition.
- Added [`isInitialized`](#https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#isinitialized) method obtained from `useInitialize` hook for checking if the SDK is initialized
- [`getRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getremotevisitordata) method now accepts new boolean parameter `isUniqueIdentifier` to obtain all linked visitors data when working with [Cross-device experimentation](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#cross-device-experimentation)
- Miscellaneous [Cross-device experimentation](https://developers.kameleoon.com/core-concepts/cross-device-experimentation) improvements

### Patch Changes

- Improved visits data collection logic
- Fixed the issue with when `_` variable was transformed to duplicate identifier `a` causing `TypeError`
- Kameleoon network request headers are now properly used
- `Browser` condition could throw an error when the browser version wasn't specified on Kameleoon Platform
- `Geolocation` condition is now case insensitive
- SDK Core version is no more sent with tracking
- Updated dependencies
  - @kameleoon/javascript-sdk@3.1.0

## 9.0.1 (2024-02-21)

### Patch Changes

- `<KameleoonProvider />` React dependency is now properly used
- Convert dependency on [JavaScript SDK](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/js-sdk/) from peer to concrete

## 9.0.0 (2024-02-16)

### Breaking Changes

- `getReactVisitorCode` method and according hook were removed, instead use [`getVisitorCode`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getvisitorcode) method
- `getReactNativeVisitorCode` method and according hook were removed, instead use [`getVisitorCode`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getvisitorcode) method
- Previously deprecated `getBrowserVisitorCode` method and according hook were removed
- Most hooks were merged and methods were grouped by hooks:
  - `useAddData`/`useTrackConversion`/`useFlush`/`useRemoteData`/`useRemoteVisitorData`/`useWarehouseAudience` -> `useData` ,containing all methods from the listed hooks
  - `useFeatureFlagActive`/`useFeatureFlags`/`useFeatureVariable`/`useFeatureVariables`/`useFeatureFlagVariationKey`/`useVisitorFeatureFlags`/`useEngineTrackingCode` -> `useFeatureFlag`, containing all methods from the listed hooks
  - `useConfigurationUpdate` -> `useInitialize`, containing `onConfigurationUpdate` and `initialize` methods
  - `useSetLegalConsent` -> `useVisitorCode`, containing `getVisitorCode` and `setLegalConsent` methods
- React SDK for React Native now requires [providing external dependencies](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#react-native-considerations)
- React SDK no more has React Native related packages in it's dependencies

### Features

- Added [External Dependencies](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#external-dependencies) capabilities

### Patch Changes

- Updated dependencies
  - @kameleoon/javascript-sdk@3.0.0

## 8.5.2 (2024-02-07)

### Bug fixes

- Tracking wasn't sent if consent is required

## 8.5.1 (2024-01-29)

### Bug fixes

- Context binding in [useSetLegalConsent](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk/#usesetlegalconsent) hook

## 8.5.0 (2024-01-18)

### Bug fixes

- SDK threw an error reading storage when migrating from older SDK versions
- React Native visitor code couldn't be read from storage in certain React Native versions
- React SDK request headers caused CORS errors

### Features

- Added new SDK `configuration` parameter [`requestTimeout`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/js-sdk/#1-initializing-the-kameleoon-client), which defines maximum time in _milliseconds_ after which any SDK network request will fail

## 8.4.1 (2023-12-15)

### Bug fixes

- Fix nonce for `Conversion` data

## 8.4.0 (2023-12-12)

### Features

- Updated the `getFeatureFlagVariable` function of [useFeatureVariable](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usefeaturevariable) hook to return an object of type `FeatureFlagVariableType`
- Enhanced the `getFeatureFlagVariables`function of [useFeatureVariables](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usefeaturevariables) hook to include the `key` field in its return value.

### Bug fixes

- Custom Data mapping identifier wasn't tracked correctly

## 8.3.0 (2023-12-11)

### Features

- [flush](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useflush) and [trackConversion](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usetrackconversion) methods now have a new optional parameter `isUniqueIdentifier` used for extra [Kameleoon Cross-device Experimentation](https://developers.kameleoon.com/core-concepts/cross-device-experimentation) capabilities

### Bug fixes

- Targeting data cleanup caused `TypeError`

## 8.2.1 (2023-12-04)

### Bug fixes

- Client cookie is now set properly

## 8.2.0 (2023-11-30)

### Features

- [CustomData session merging](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#using-custom-data-for-session-merging) abilities for [Kameleoon Cross-device Experimentation](https://developers.kameleoon.com/core-concepts/cross-device-experimentation)

## 8.1.0 (2023-11-24)

### Features

- Added `useSetLegalConsent` hook to determine the types data Kameleoon includes in tracking requests. This helps you adhere to legal and regulatory requirements while responsibly managing visitor data. You can find more information in the [Consent management policy](https://help.kameleoon.com/consent-management-policy).

## 8.0.0 (2023-11-16)

### Breaking change

- SDK stopped the support of the following methods were:
  - `useExperiments`
  - `useVisitorExperiments`
  - `useTriggerExperiment`
- Previously deprecated `useFlushData` hook was removed
- [`createClient`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#createclient) method parameters are now an object with type [`SDKParameters`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#parameters)
- [`useRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useremotevisitordata) hook callback parameters was changed to an object with type [`RemoteVisitorDataParamsType`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/js-sdk#parameters-12) that has a new `filters` field

### Features

- New hook with a method for retrieving multiple feature flag variables - [`useFeatureFlagVariables`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usefeatureflagvariables)
- [`useRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useremotevisitordata) callback is now capable of retrieving information from up to 25 visits along with some additional visit information configured by new `filters` field
- [`PageView`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#pageview) data items are now stored by unique URL with timestamps of visits - each visitor can have several `PageView` URLs stored
- [`Conversion`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#conversion) data items are now stored as a list of conversion for each visitor
- [Cross Device Synchronization](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#synchronizing-custom-data-across-devices) was improved - [`CustomData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#customdata) with `Visitor` scope is always re-sent with tracking calls allowing seamless synchronization using [`getRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getremotevisitordata)
- [New targeting conditions](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#list-of-supported-targeting-conditions) are now available (some of them may require [`getRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getremotevisitordata) pre-loaded data)
  - `Browser Cookie`
  - `Application Version`
  - `Operating System`
  - `IP Geolocation`
  - `Segment`
  - `Target Feature Flag`
  - `Previous Page`
  - `Number of Page Views`
  - `Time since First Visit`
  - `Time since Last Visit`
  - `Number of Visits Today`
  - `Total Number of Visits`
  - `New or Returning Visitor`
- New Kameleoon Data types were introduced:
  - [`ApplicationVersion`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#applicationversion)
  - [`Cookie`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#cookie)
  - [`OperatingSystem`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#operatingsystem)
  - [`GeolocationData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#geolocationdata)

### Bug fixes

- `SDKParameters` type is now correctly exported from SDK
- SDK Polling re-tries mechanism was optimized - SDK will now try to obtain configuration again during the next poll after 3 failed configuration loading attempts
- [`onConfigurationUpdate`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useconfigurationupdate) callback now fires on successful configuration update in storage (previously fired after network configuration retrieval)

## 7.6.1 (2023-10-20)

### Bug fixes

- Fix previous version deploy issue

## 7.6.0 (2023-10-20)

### Features

- Added new hook [`useVisitorWarehouseAudience`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usevisitorwarehousedata)

## 7.5.4 (2023-10-11)

### Bug fixes

- Storage data parse overhead optimization

## 7.5.3 (2023-09-05)

### Bug fixes

- `UserAgent` was not exported from SDK

## 7.5.2 (2023-08-31)

### Bug fixes

- `Custom Data Condition` now handles index `0` properly

## 7.5.1 (2023-08-25)

### Bug fixes

- Multiple `Real Time Update` connections are no longer created
- `Custom Data Condition` now handles all exceptions properly

## 7.5.0 (2023-08-11)

### Bug fixes

- Empty Custom Data is now sending activity tracking event

### Features

- Added [Cross Device Custom Data Synchronization](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#cross-device-custom-data-synchronization) capabilities

## 7.4.1 (2023-07-26)

### Bug fixes

- The returned function of [`useFlush`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useflush) now sends offline tracking requests even if there's no new data to track.
- Timestamps for offline requests are set correctly.

## 7.4.0 (2023-07-21)

### Features

- `useFlushData` has been deprecated in favor of [`useFlush`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useflush).
- The function returned from [`useFlush`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useflush) sends failed tracking requests that were stored locally during the offline mode at first and then proceeds with the latest request.

## 7.3.2 (2023-07-17)

### Bug fixes

- Typescript `.d.ts` files have proper relative path resolution.
- Unused tracking parameters are no longer sent.
- Real-time update events now get the latest configuration.
- Added exports for following types:
  - [`ExperimentType`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useexperiments)
  - [`FeatureFlagType`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usefeatureflags)
  - [`FeatureVariableResultType`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usefeaturevariable)
  - [`KameleoonDataType`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useremotevisitordata)

### Features

- When the client is initialized in offline mode, in case of network issues failed tracking requests made by returned functions of [`useFlushData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useflushdata), [`useTrackConversion`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usetrackconversion), [`useTriggerExperiment`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usetriggerexperiment), [`useFeatureFlagVariationKey`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usefeatureflagvariationkey) or [`useFeatureVariable`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usefeaturevariable) will be sent anew once the client is back online.
- Minor optimization for checking [targeting conditions](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#list-of-supported-targeting-conditions) of a segment.

## 7.3.1 (2023-06-30)

### Bug fixes

- Tracking data duplication

## 7.3.0 (2023-06-28)

### Bug fixes

- Improve error handling for [`getRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#obtain-custom-data-from-kameleoon-data-api)
- Visitor code is now validated correctly in every method that uses it.
- Result bundle is now compatible with systems not using `ESM` and `CommonJS`. It's minimized and uses code splitting for `crypto-js` `sha256` function.
- Parameter `revenue` for [`Conversion`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#conversion) is now optional.
- Each visitor can now only have one type of associated [`Kameleoon Data Type`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#data-types), except for [`CustomData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#customdata), that a visitor can have one per `customDataIndex`.

### Features

- New conditions are now supported: `Device`, `Browser`, `SDKLanguage`, `Page Title`, `Page View`, `Visitor Code`, `Conversion`. See the [full list of supported conditions](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#list-of-supported-targeting-conditions).
- [`Browser`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#browser) now has new optional parameter `version`.
- [`flushData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#flush-tracking-data) `visitorCode` parameter is now optional.
- Custom data that is marked as `local only` on Kameleoon Platform is now only used for targeting (not flushed with tracking requests).

## 7.2.4 (2023-06-01)

### Bug fixes

- Empty visitor code is no more allowed
- Incorrect tracking body upon triggering an experiment

## 7.2.3 (2023-05-24)

### Bug fixes

- Fixed issue for sending unique `Nonce` parameter on tracking requests

## 7.2.2 (2023-05-21)

### Bug fixes

- [`useRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#obtain-custom-data-from-kameleoon-data-api) current visits not being up-to-date

## 7.2.1 (2023-05-20)

### Bug fixes

- [`useRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#obtain-custom-data-from-kameleoon-data-api) data parse error

## 7.2.0 (2023-05-19)

### Features

- New hook [`useRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#obtain-custom-data-from-kameleoon-data-api)
- New offline mode for [`useInitialize`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#initialize-kameleoon-client) method

## 7.1.2 (2023-04-24)

### Bug fixes

- Tracking feature flag rule with variation `off` wasn't displayed on result page

## 7.1.1 (2023-04-22)

### Bug fixes

- core dependency fix

## 7.1.0 (2023-04-21)

### Features

- New hook [`useEngineTrackingCode`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#sending-exposure-events-to-external-tools)

## 7.0.0 (2023-04-14)

### Breaking change

- Error handling has changed, hook do not return `error` object anymore. See [details](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#error-handling)

### Features

- Added hook [`useExperimentVariationData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useexperimentvariationdata)
- Targeting data cleanup interval is now [`optional`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#1-initializing-the-kameleoon-client) and not set by default
- [`getFeatureFlagVariationKey`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#get-variation-key-for-a-certain-feature-flag), [`getFeatureFlagVariable`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#get-a-variable-of-a-certain-feature-flag), [`isFeatureFlagActive`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#check-if-the-feature-is-active-for-visitor) methods returned from according hooks do not check previously allocated variation, rather recalculate it each time. Same methods now send tracking request for any rule type (previously only for `Experementation` rule)

### Bug fixes

- `variationId` is undefined when using feature flag related methods

## 6.1.3 (2023-04-05)

### Bug fixes

- export missing hooks

## 6.1.2 (2023-04-05)

### Bug fixes

- react sdk tests

## 6.1.1 (2023-03-24)

### Bug fixes

- change broken dependency

## 6.1.0 (2023-03-22)

### Features

- License changed from `GPL3.0` to `ISC`

## 6.0.0 (2023-03-21)

### Features

- Feature Flag v2 support
- Improved stability

### Breaking change

- All `HOCs`,`Feature` render props, `compose` were removed
- All data types hooks were removed: `useDevice`, `useConversion`, `useCustomData`, `usePageView`, `useBrowser`
- Renamed hooks:
  - `useFlush` -> `useFlushData`
  - `useActivateFeature` -> `useFeatureActive`
  - `useVisitorCode` -> `useBrowserVisitorCode`
  - `useAsyncVisitorCode` -> `useNativeVisitorCode`
  - `useRetrieveDataFromRemoteSource` -> `useRemoteData`
- Removed hooks:
  - `useVariationAssociatedData`
  - `useRunWhenReady`
  - `useFeature`
- Added hooks:
  - `useInitialize`
  - `useConfigurationUpdate`
  - `useFeatureFlags`
  - `useVisitorFeatureFlags`
  - `useExperiments`
  - `useVisitorExperiments`
  - `useFeatureFlagVariationKey`

## 5.0.0 (2023-02-03)

### Bug fixes

- remove xhr

### Breaking change

- visitor code is now mandatory for hasFeature and useFeature

## 4.1.1 (2022-11-01)

### Bug fixes

- update tests

## 4.1.0 (2022-10-13)

### Features

- react native async storage

## 4.0.3 (2022-09-05)

### Bug fixes

- rerender error

## 4.0.2 (2022-09-02)

### Bug fixes

- max depth error

## 4.0.1 (2022-08-23)

### Bug fixes

- useDevice hook description

## 4.0.0 (2022-08-18)

### Features

- swap feature arguments
- add device custom data

## 3.0.0 (2022-07-01)

### Features

- error handling

### Breaking change

- `useFeature` hook and `withFeature` high-order component can no longer be used the same way

## 2.1.0 (2022-04-18)

### Features

- retrieve data from remote source

## 2.0.0 (2022-02-24)

### Features

- add multi environment support

### Breaking change

- variableKeys became an object and no longer can be used as an array or a string

## 1.2.2 (2022-02-06)

### Bug fixes

- bundle source and internal changelog

## 1.2.1 (2022-02-06)

### Bug fixes

- decrease bundle size

## 1.2.0 (2022-01-27)

### Bug Fixes

- linting issues

## 1.1.0 (2022-01-26)

### Bug Fixes

- package json

## 1.0.0 (2022-01-20)

### Bug Fixes

- linting and addData

### Features

- add HOCs
- add render props
- test react sdk methods
- complete the documentation
- add createClient, provider and hooks
