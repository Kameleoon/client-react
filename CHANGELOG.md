# Change Log

All notable changes to this project will be documented in this file.

[Project Homepage](https://developers.kameleoon.com/react-js-sdk.html)



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
