# Change Log

All notable changes to this project will be documented in this file.

[Project Homepage](https://developers.kameleoon.com/react-js-sdk.html)

# 8.2.0 (2023-11-30)


### Features

* [CustomData session merging](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#using-custom-data-for-session-merging) abilities for [Kameleoon Cross-device Experimentation](https://developers.kameleoon.com/core-concepts/cross-device-experimentation)

# 8.1.0 (2023-11-24)

### Features

* Added `useSetLegalConsent` hook to determine the types data Kameleoon includes in tracking requests. This helps you adhere to legal and regulatory requirements while responsibly managing visitor data. You can find more information in the [Consent management policy](https://help.kameleoon.com/consent-management-policy).
* Migration to new CC end-point

# 8.0.0 (2023-11-16)


### Breaking change

* SDK stopped the support of Web Experiments, the following hooks were removed:
  - `useExperiments`
  - `useVisitorExperiments`
  - `useTriggerExperiment`
* Previously deprecated `useFlushData` hook was removed
* [`createClient`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#createclient) method parameters are now an object with type [`SDKParameters`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#parameters)
* [`useRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useremotevisitordata) hook callback parameters was changed to an object with type [`RemoteVisitorDataParamsType`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/js-sdk#parameters-12) that has a new `filters` field


### Features

* New hook with a method for retrieving multiple feature flag variables - [`useFeatureFlagVariables`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/nodejs-sdk#usefeatureflagvariables)
* [`useRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useremotevisitordata) callback is now capable of retrieving information from up to 25 visits along with some additional visit information configured by new `filters` field
* [`PageView`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#pageview) data items are now stored by unique URL with timestamps of visits - each visitor can have several `PageView` URLs stored
* [`Conversion`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#conversion) data items are now stored as a list of conversion for each visitor
* [Cross Device Synchronization](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#synchronizing-custom-data-across-devices) was improved - [`CustomData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#customdata) with `Visitor` scope is always re-sent with tracking calls allowing seamless synchronization using [`getRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getremotevisitordata)
* [New targeting conditions](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#list-of-supported-targeting-conditions) are now available (some of them may require [`getRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#getremotevisitordata) pre-loaded data)
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
* New Kameleoon Data types were introduced:
  - [`ApplicationVersion`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#applicationversion)
  - [`Cookie`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#cookie)
  - [`OperatingSystem`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#operatingsystem)
  - [`GeolocationData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#geolocationdata)


### Bug fixes

* `SDKParameters` type is now correctly exported from SDK
* SDK Polling re-tries mechanism was optimized - SDK will now try to obtain configuration again during the next poll after 3 failed configuration loading attempts
* |JS| [`onConfigurationUpdate`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/js-sdk#onconfigurationupdate) callback now fires on successful configuration update in storage (previously fired after network configuration retrieval)
* |NodeJS| [`onConfigurationUpdate`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/nodejs-sdk#onconfigurationupdate) callback now fires on successful configuration update in storage (previously fired after network configuration retrieval)
* |React| [`onConfigurationUpdate`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useconfigurationupdate) callback now fires on successful configuration update in storage (previously fired after network configuration retrieval)

# 7.6.1 (2023-10-20)


### Bug fixes

* Fix previous version deploy issue

# 7.6.0 (2023-10-20)


### Features

* Added new hook [`useVisitorWarehouseAudience`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usevisitorwarehousedata)

# 7.5.4 (2023-10-11)


### Bug fixes

* Storage data parse overhead optimization

# 7.5.3 (2023-09-05)


### Bug fixes

* `UserAgent` was not exported from SDK

# 7.5.2 (2023-08-31)


### Bug fixes

* `Custom Data Condition` now handles index `0` properly

# 7.5.1 (2023-08-25)


### Bug fixes

* Multiple `Real Time Update` connections are no longer created
* `Custom Data Condition` now handles all exceptions properly

# 7.5.0 (2023-08-11)


### Bug fixes

* Empty Custom Data is now sending activity tracking event

### Features

* Added [Cross Device Custom Data Synchronization](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#cross-device-custom-data-synchronization) capabilities

# 7.4.1 (2023-07-26)


### Bug fixes

* The returned function of [`useFlush`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useflush) now sends offline tracking requests even if there's no new data to track.
* Timestamps for offline requests are set correctly.

# 7.4.0 (2023-07-21)


### Features

- `useFlushData` has been deprecated in favor of [`useFlush`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useflush).
- The function returned from [`useFlush`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useflush) sends failed tracking requests that were stored locally during the offline mode at first and then proceeds with the latest request.

# 7.3.2 (2023-07-17)


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

# 7.3.1 (2023-06-30)


### Bug fixes

* Tracking data duplication

# 7.3.0 (2023-06-28)


### Bug fixes

- Improve error handling for [`getRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#obtain-custom-data-from-kameleoon-data-api)
- Visitor code is now validated correctly in every method that uses it.
- Result bundle is now compatible with systems not using `ESM` and `CommonJS`. It's minimized and uses code splitting for `crypto-js` `sha256` function.
- Parameter `revenue` for [`Conversion`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#conversion) is now optional.
- Each visitor can now only have one type of associated [`Kameleoon Data Type`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#data-types), except for [`CustomData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/nodejs-sdk#customdata), that a visitor can have one per `customDataIndex`.

### Features

- New conditions are now supported: `Device`, `Browser`, `SDKLanguage`, `Page Title`, `Page View`, `Visitor Code`, `Conversion`. See the [full list of supported conditions](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#list-of-supported-targeting-conditions).
- [`Browser`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#browser) now has new optional parameter `version`.
- [`flushData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#flush-tracking-data) `visitorCode` parameter is now optional.
- Custom data that is marked as `local only` on Kameleoon Platform is now only used for targeting (not flushed with tracking requests).

# 7.2.4 (2023-06-01)


### Bug fixes

* Empty visitor code is no more allowed
* Incorrect tracking body upon triggering an experiment

# 7.2.3 (2023-05-24)


### Bug fixes

* Fixed issue for sending unique `Nonce` parameter on tracking requests

# 7.2.2 (2023-05-21)


### Bug fixes

* [`useRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#obtain-custom-data-from-kameleoon-data-api) current visits not being up-to-date

# 7.2.1 (2023-05-20)


### Bug fixes

* [`useRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#obtain-custom-data-from-kameleoon-data-api) data parse error


# 7.2.0 (2023-05-19)


### Features

* New hook [`useRemoteVisitorData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#obtain-custom-data-from-kameleoon-data-api)
* New offline mode for [`useInitialize`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#initialize-kameleoon-client) method

# 7.1.2 (2023-04-24)


### Bug fixes

* Tracking feature flag rule with variation `off` wasn't displayed on result page

# 7.1.1 (2023-04-22)


### Bug fixes

* core dependency fix

# 7.1.0 (2023-04-21)


### Features

* New hook [`useEngineTrackingCode`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#sending-exposure-events-to-external-tools)


# 7.0.0 (2023-04-14)

### Breaking change

* Error handling has changed, hook do not return `error` object anymore. See [details](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#error-handling)

### Features

* Added hook [`useExperimentVariationData`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#useexperimentvariationdata)
* Targeting data cleanup interval is now [`optional`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#1-initializing-the-kameleoon-client) and not set by default
* [`getFeatureFlagVariationKey`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#get-variation-key-for-a-certain-feature-flag), [`getFeatureFlagVariable`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#get-a-variable-of-a-certain-feature-flag), [`isFeatureFlagActive`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#check-if-the-feature-is-active-for-visitor) methods returned from according hooks do not check previously allocated variation, rather recalculate it each time. Same methods now send tracking request for any rule type (previously only for `Experementation` rule)


### Bug fixes

* `variationId` is undefined when using feature flag related methods

# 6.1.3 (2023-04-05)


### Bug fixes

* export missing hooks

# 6.1.2 (2023-04-05)


### Bug fixes

* react sdk tests

# 6.1.1 (2023-03-24)


### Bug fixes

* change broken dependency

# 6.1.0 (2023-03-22)


### Features

- License changed from `GPL3.0` to `ISC`

# 6.0.0 (2023-03-21)

### Features

* Feature Flag v2 support
* Improved stability

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

# 5.0.0 (2023-02-03)

### Bug fixes

* remove xhr

### Breaking change

* visitor code is now mandatory for hasFeature and useFeature

# 4.1.1 (2022-11-01)


### Bug fixes

* update tests

# 4.1.0 (2022-10-13)


### Features

* react native async storage

# 4.0.3 (2022-09-05)


### Bug fixes

* rerender error

# 4.0.2 (2022-09-02)


### Bug fixes

* max depth error

# 4.0.1 (2022-08-23)


### Bug fixes

* useDevice hook description

# 4.0.0 (2022-08-18)


### Features

* swap feature arguments
* add device custom data

# 3.0.0 (2022-07-01)


### Features

* error handling

### Breaking change

* `useFeature` hook and `withFeature` high-order component can no longer be used the same way

# 2.1.0 (2022-04-18)


### Features

* retrieve data from remote source

# 2.0.0 (2022-02-24)


### Features

* add multi environment support

### Breaking change

* variableKeys became an object and no longer can be used as an array or a string

# 1.2.2 (2022-02-06)


### Bug fixes

* bundle source and internal changelog

# 1.2.1 (2022-02-06)


### Bug fixes

* decrease bundle size

# 1.2.0 (2022-01-27)


### Bug Fixes

* linting issues


# 1.1.0 (2022-01-26)


### Bug Fixes


* package json

# 1.0.0 (2022-01-20)


### Bug Fixes

* linting and addData

### Features

* add HOCs
* add render props
* test react sdk methods
* complete the documentation
* add createClient, provider and hooks
