# Kameleoon React SDK

### Introduction
This repository is a Kameleoon React SDK which is used to run experiments and get feature flag status and its variables. Integration of this SDK into web application is easy, and its footprint is low.

### Requirements
This React SDK requires `React 16.8.0+`

### Installation
Installing the React SDK can be directly achieved through an `npm` or `yarn` module:

**npm**
```
npm install @kameleoon/react-sdk
```

**yarn**
```
yarn add @kameleoon/react-sdk
```

### Usage

## `createClient`
A `KameleoonClient` created using `createClient()` to run experiments and retrive the status of feature flag and its variables.

#### Arguments
- `clientParams: ICreateClientParams`
  - `siteCode: string` - code of the website on which experiments run. This unique code id can be found in our platform.
  - `visitorCode: string` (optional) - visitor code is an optional parameter, if it is used, it automatically create custom data to check if visitor is targeted.
  - `option: SDKConfiguration` (optional) - JSON object filled with configuration parameters.
    - `actions_configuration_refresh_interval: number` (optional) - an option specifying the refresh interval, in minutes, of the configuration for experiments and feature flags (the active experiments and feature flags are fetched from the Kameleoon servers). It means that once you launch an experiment, pause it, or stop it the changes can take (at most) the duration of this interval to be propagated in production to your servers. If not specified, the **default interval is 60 minutes**.
    - `visitor_data_maximum_size: number` (optional) - an option specifying the maximum amount of memory that [the map holding all the visitor data](https://developers.kameleoon.com/javascript-sdk.html#technical-considerations) (in particular custom data) can take (in MB on the browser LocalStorage). If specified, **max size is 5MB**. If not specified, the **default size is 1MB**.
    - `environment: string` (optional) - an option specifying which feature flag configuration will be used, by default each feature flag is split into **production**, **staging**, **development**. If not specified, will be set to **default value** of **production**.

    **NOTICE:** make sure not to use several client instances in one application with different `environment` as it is not fully supported yet and might lead to local storage configuration being overwritten and cause bugs.

#### Returns
- A `KameleoonClient` instance.

#### Example
```jsx
import { createClient } from '@kameleoon/react-sdk';

const client = createClient({
  siteCode: 'dfl102d38e',
  visitorCode: '280295',
  options: {
    visitor_data_maximum_size: 1,
    actions_configuration_refresh_interval: 60,
    environment: 'staging'
  }
});
```

## `<KameleoonProvider>`
Use this provider in root level by wrapping your app to gain an access to `KameleoonClient`. This ensures your app does not flicker due to flag changes at startup time.

#### Props
- `children: ReactNode` - child elements of the provider.
- `client: KameleoonClient` - `KameleoonClient` instance created by `createClient()`.

#### Example
```jsx
import { KameleoonProvider, createClient } from '@kameleoon/react-sdk';

const client = createClient({
  siteCode: 'dfl102d38e'
  visitorCode: '280295',
  options: {
    visitor_data_maximum_size: 1,
    actions_configuration_refresh_interval: 60,
    environment: 'staging'
  }
});

function AppWrapper(): JSX.Element {
  return (
    <KameleoonProvider client={client}>
      <App />
    </KameleoonProvider>
  )
}
```

## Kameleoon client
These Hook and HOC validate whether they are used within `KameleoonProvider` and provide an access to `KameleoonClient`.

### `useKameleoon`
#### Returns
- Returns `object: KameleoonHookResult`:
  - `client: KameleoonClient` - an instance of `KameleoonClient` taken from context.

#### Example
```jsx
import { useKameleoon } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { client } = useKameleoon();

  ...
}
```

### `withKameleoon`
#### Returns
- `Component: React.Component` - component which will be enhanced with the prop `client: KameleoonClient`.

```jsx
import { withKameleoon } from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { client } = this.props;
  }

  ...
}

export default withKameleoon(MyComponent);
```

## LocalStorage Key
- `KAMELEOON_SDK_LOCAL_STORAGE_KEY` - is a constant which used as a key in `LocalStorage` to store a `kameleoonTargetingData` and `kameleoonConfiguration`.

```jsx
import { KAMELEOON_SDK_LOCAL_STORAGE_KEY } from '@kameleoon/react-sdk'
```

## Obtain status and variables of feature flag
Returns the status of a feature flag and its variables.

#### Arguments
- `featureParams: IFeatureParams`
  - `featureKey: string | number` - unique identifier or key of the feature you want to expose to a user.
  - `variableKeys: VariableKeysType` - keys for the variables on different environments.
  - `visitorCode: string` (optional) - unique identifier of the user.

#### Returns
- `featureResult: IFeature`
  - `isActive: boolean` - feature flag status.
  - `variables: FeatureVariableType[]` - feature flag variables.


In this examples, if the feature flag with the `red-button` key is enabled, theme of button will set to red.
### `useFeature`
#### Example
```jsx
import { Button } from '@kameleoon/ui';
import { useFeature, useVisitorCode } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { getVisitorCode } = useVisitorCode();
  const { isActive } = useFeature({
    featureKey: 'red-button',
    variableKeys: { production: 'red-button' },
    visitorCode: getVisitorCode('example.com'),
  });

  return <Button theme={isActive ? 'red' : 'green'} />;
}
```

### `withFeature`
#### Example
```jsx
import { Button } from '@kameleoon/ui';
import { withFeature } from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  render() {
    const { isActive, variables } = this.props;
    return <Button theme={isActive ? 'red' : 'green'} />;
  }
}

export default withFeature({
  featureKey: 'red-button',
  variableKeys: { production: 'red-button' },
  visitorCode: '280295'
})(MyComponent);
```

### `Feature` render props
#### Props
- `children: (featureResult: IFeature) => ReactNode` - child elements of `Feature`.
- `featureKey: string | number` - unique identifier or key of the feature you want to expose to a user.
- `variableKeys: VariableKeysType` - keys for the variables on different environments.
- `visitorCode: string` - unique identifier of the user.

#### Returns
- A wrapped component with the following props:
  - `isActive: boolean` - feature flag status.
  - `variables: FeatureVariableType[]` - feature flag variables.

#### Example
```jsx
import { Button } from '@kameleoon/ui';
import { Feature } from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  render() {
    const { isActive, variables } = this.props;
    return <Button theme={isActive ? 'red' : 'green'} />;
  }
}

class MyCompWrapper extends React.Component { 
  render() {
    return(
      <Feature featureKey="red-button" variableKeys={{production: "red-button"}} visitorCode="280295">
        (({isActive, variables}) => (
          <MyComponent isActive={isActive} variables={variables} />
        ))
      </Feature>;
    )
  }
}
```

## Obtain visitorCode
A callback function `getVisitorCode()` retrieves the Kameleoon `visitorCode` for the current visitor. This is especially important when using Kameleoon in a mixed front-end and back-end environment, where user identification consistency must be guaranteed. The implementation logic is described here:

1. First we check if a `kameleoonVisitorCode` cookie can be found. If so, we will use this as the visitor identifier.

2. If no cookie is found, we either randomly generate a new identifier, or use the `defaultVisitorCode` argument as identifier if it is passed. This allows our customers to use their own identifiers as visitor codes, should they wish to. This can have the added benefit of matching Kameleoon visitors with their own users without any additional look-ups in a matching table.

3. In any case, the JavaScript `kameleoonVisitorCode` cookie is set with the value. Then this identifier value is finally returned by the method.

For more information, refer to [this article](https://developers.kameleoon.com/back-front-bridge.html).

##### Please Note:
_If you provide your own `visitorCode`, its uniqueness must be guaranteed on your end - the SDK cannot check it. Also note that the length of `visitorCode` is **limited to 255 characters**. Any excess characters will throw an exception._

#### `getVisitorCode()`

##### Arguments
- `topLevelDomain: string` - current top level domain for the concerned site (this information is needed to set the corresponding cookie accordingly, on the top level domain).
- `defaultVisitorCode: string` (optional) - This parameter will be used as the `visitorCode` if no existing `kameleoonVisitorCode` cookie is found on the request. This field is optional, and by default a random `visitorCode` will be generated.

##### Returns
- `visitorCode: string` - a visitorCode that will be associated with this particular user and should be used with most of the methods of the SDK.

### `useVisitorCode`
#### Returns
- A callback function `getVisitorCode()`.

#### Example
```jsx
import { useEffect } from 'react';
import { uuidv4 } from 'uuid';
import { useVisitorCode } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { getVisitorCode } = useVisitorCode();

  useEffect(() => {
    // Without defaultVisitorCode argument
    const visitorCode = getVisitorCode('example.com');

    // With defaultVisitorCode argument
    const visitorCode = getVisitorCode('example.com', uuidv4());
  }, []);

  ...
}
```
### `withVisitorCode`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `getVisitorCode()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import { uuidv4 } from 'uuid';
import { withVisitorCode } from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { getVisitorCode } = this.props;

    // Without defaultVisitorCode argument
    const visitorCode = getVisitorCode('example.com');

    // With defaultVisitorCode argument
    const visitorCode = getVisitorCode('example.com', uuidv4());
  }

  ...
}

export default withVisitorCode(MyComponent);
```

## Triggering an experiment
A callback function `getVariationId()` takes `visitorCode` and `experimentId` as mandatory arguments to register a variation for a given user.

If such a user has never been associated with any variation, the SDK returns a randomly selected variation. If a user with a given `visitorCode` is already registered with a variation, it will detect the previously registered variation and return the `variationId`.

You have to make sure that proper error handling is set up in your code as shown in the example to the right to catch potential exceptions.

##### Please Note:
_By convention, the reference (original variation) always has an ID equal to **0**._

#### Exceptions Thrown
- `KameleoonException.NotTargeted` - Exception indicating that the current visitor / user did not trigger the required targeting conditions for this experiment. The targeting conditions are defined via Kameleoon's segment builder.
- `KameleoonException.NotActivated` - Exception indicating that the current visitor / user triggered the experiment (met the targeting conditions), but did not activate it. The most common reason for that is that part of the traffic has been excluded from the experiment and should not be tracked.
- `KameleoonException.ExperimentConfigurationNotFound` - Exception indicating that the requested experiment ID has not been found in the internal configuration of the SDK. This is usually normal and means that the experiment has not yet been started on Kameleoon's side (but code triggering / implementing variations is already deployed on the web-application's side).

#### `getVariationId()`
##### Arguments
- `visitorCode: string` - unique identifier of the user
- `experimentId: number` - unique identifier of the experiment you want to expose to a user

##### Returns
- `variationId: number` - ID of the variation that is registered for a given visitorCode. By convention, the reference (original variation) always has an ID equal to **0**.

### `useTriggerExperiment`
#### Returns
- A callback function `getVariationId()`.

#### Example
```jsx
import { useEffect } from 'react';
import { useTriggerExperiment, useVisitorCode } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { getVariationId } = useTriggerExperiment();
  const { getVisitorCode } = useVisitorCode();

  useEffect(() => {
    const visitorCode = getVisitorCode('example.com');
    const experimentId = 12341;

    const variationId = getVariationId(visitorCode, experimentId);
  }, []);

  ...
}
```

### `withTriggerExperiment`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `getVariationId()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import { withVisitorCode, withTriggerExperiment, compose } from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { getVisitorCode, getVariationId } = this.props;
    const visitorCode = getVisitorCode('example.com');
    const experimentId = 230243;

    const variationId = getVariationId(visitorCode, experimentId);
  }

  ...
}

export default compose(withVisitorCode, withTriggerExperiment)(MyComponent);
```

## Activate feature
A callback function `hasFeature()` which validates if user has been associated with this feature. If it is fails, callback returns value **false**, otherwise **true**.

If feature flag is not activated, `KameleoonException.FeatureConfigurationNotFound` exception will be thrown. Please, make sure to handle properly the error.

#### Exceptions Thrown
- `KameleoonException.NotTargeted` - Exception indicating that the current visitor / user did not trigger the required targeting conditions for this feature. The targeting conditions are defined via Kameleoon's segment builder.
- `KameleoonException.FeatureConfigurationNotFound` - Exception indicating that the requested feature ID has not been found in the internal configuration of the SDK. This is usually normal and means that the feature flag has not yet been activated on Kameleoon's side (but code implementing the feature is already deployed on the web-application's side).

#### `hasFeature()`
##### Arguments
- `visitorCode: string` - unique identifier of the user.
- `featureKey: string | number` - unique identifier or key of the feature you want to expose to a user.

##### Returns
- A `value: boolean` of the feature that is registered for a given `visitorCode`.

### `useActivateFeature`
#### Returns
- A callback function `hasFeature()`.

#### Example
```jsx
import { useEffect } from 'react';
import { useActivateFeature, useVisitorCode } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { hasFeature } = useActivateFeature();
  const { getVisitorCode } = useVisitorCode();

  useEffect(() => {
    const visitorCode = getVisitorCode('example.com');
    const featureKey = 'example-feature-key';

    const feature = hasFeature(visitorCode, featureKey);
  }, []);

  ...
}
```

###  `withActivateFeature`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `hasFeature()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import { withVisitorCode, withActivateFeature, compose } from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { getVisitorCode, hasFeature } = this.props;
    const visitorCode = getVisitorCode('example.com');
    const featureKey = 'example-feature-key';

    const feature = hasFeature(visitorCode, featureKey);
  }

  ...
}

export default compose(withVisitorCode, withActivateFeature)(MyComponent);
```

## Obtain variation associated data
A callback function `getVariationAssociatedData()` which retrieves JSON data assiciated with a variation. The JSON data usually represents some metadata of the variation, and can be configured on our web application interface or via our Automation API.

This calllback function takes the `variationId` as a parameter and will return the data as a JavaScript object. It will throw an exception `(KameleoonException.VariationConfigurationNotFound)` if the `variationId` is wrong or corresponds to an experiment that is not yet online.

#### Exceptions Thrown
- `KameleoonException.VariationConfigurationNotFound` - Exception indicating that the requested variation ID has not been found in the internal configuration of the SDK. This is usually normal and means that the variation's corresponding experiment has not yet been activated on Kameleoon's side.

#### `getVariationAssociatedData()`

##### Arguments
- `variationId: number` - unique identifier of the variation you want to obtain associated data for.

##### Returns
- Data associated with this `variationId`.

### `useVariationAssociatedData`
#### Returns
- A callback function `getVariationAssociatedData()`.

#### Example
```jsx
import { useEffect } from 'react';
import { useVariationAssociatedData } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { getVariationAssociatedData } = useVariationAssociatedData();

  useEffect(() => {
    const variationId = 280295;
    const data = getVariationAssociatedData(variationId);
  }, []);

  ...
}
```
### `withVariationAssociatedData`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `getVariationAssociatedData()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import { withVariationAssociatedData } from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { getVariationAssociatedData } = this.props;
    const variationId = 280295;
    const data = getVariationAssociatedData(variationId);
  }

  ...
}

export default withVariationAssociatedData(MyComponent);
```

## Obtain feature variable
A callback function `getFeatureVariable()` which retrieves a feature variable.

This callback function takes two input parameters: `featureKey` and `variableKey`. It will return the data with the expected type, as defined on the web interface. It will throw an exception `(KameleoonException.FeatureConfigurationNotFound)` if the requested feature has not been found in the internal configuration of the SDK.

#### Exceptions Thrown
- `KameleoonException.FeatureConfigurationNotFound` - Exception indicating that the requested feature ID has not been found in the internal configuration of the SDK. This is usually normal and means that the feature flag has not yet been activated on Kameleoon's side.

#### `getFeatureVariable()`

##### Arguments
- `featureKey: string | number` - unique identifier or key of the feature you want to obtain to a user.
- `variableKey: string` - key of the variable.
##### Returns
- `featureVariable: FeatureFlagVariableType` - data associated with this variable for this feature flag

### `useFeatureVariable`
#### Returns
- A callback function `getFeatureVariable()`.

#### Example
```jsx
import { useEffect } from 'react';
import { useFeatureVariable } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { getFeatureVariable } = useFeatureVariable();

  useEffect(() => {
    const featureKey = 'example-feature-key';
    const variableKey = 'example-variable-key';

    const featureVariable = getFeatureVariable(featureKey, variableKey);
  }, []);

  ...
}
```

### `withFeatureVariable`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `getFeatureVariable()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import { withFeatureVariable } from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { getFeatureVariable } = this.props;
    const featureKey = 'example-feature-key';
    const variableKey = 'example-variable-key';

    const featureVariable = getFeatureVariable(featureKey, variableKey);
  }

  ...
}

export default withFeatureVariable(MyComponent);
```

## Tracking conversion
A callback function `trackConversion()` which tracks conversion. This callback function requires `visitorCode` and `goalId` to track conversion on this particular goal. In addition, `trackConverion()` accepts third optional argument `revenue` to track revenue. The `visitorCode` is usually identical to the one that was used when triggering the experiment.

This callback function doesn't return any value. And it is non-blocking as the server call is made asynchronously.

#### `trackConversion()`
##### Arguments
- `visitorCode: string` - unique identifier of the user.
- `goalId: number` - key of the variable.
- `revenue: number` (optional) - key of the variable.

### `useTrackingConversion`
#### Returns
- A callback function `trackConversion()`;

#### Example
```jsx
import { useEffect } from 'react';
import { useTrackingConversion, useVisitorCode } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { getVisitorCode } = useVisitorCode();
  const { trackConversion } = useTrackingConversion();

  useEffect(() => {
    const visitorCode = getVisitorCode('example.com')
    const goalId = 280295;

    trackConversion(visitorCode, goalId);
  }, []);

  ...
}
```

### `withTrackingConversion`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `trackConversion()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import { withVisitorCode, withTrackingConversion, compose } from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { getVisitorCode, trackConversion } = this.props;
    const visitorCode = getVisitorCode('example.com');
    const goalId = 230234;

    trackConversion(visitorCode, goalId);
  }

  ...
}

export default compose(withVisitorCode, withTrackingConversion)(MyComponent);
```


## Add data
A callback function `addData()` which adds various data to associate this data with the current user. This callback function requires `visitorCode` as a first parameter and then accepts array of various **Data Types** allowed in Kameleoon.

Note that the `addData()` doesn't return any value and doesn't interact with the Kameleoon back-end servers by itself. Instead, all declared data is saved for further sending via the `flush()` described in the next paragraph. This reduces the number of server calls made, as data is usually grouped into a single server call triggered by the execution of `flush()`.

##### Please Note:
_The `triggerExperiment()` and `trackConversion()` callback functions also sends out previously associated data, exactly as the `flush()`._

#### `addData()`
##### Arguments
- `visitorCode: string` - unique identifier of the user.
- `dataTypes: DataInterface[]` - custom data types.

### `useAddData`
#### Returns
- A callback function `addData()`.

#### Example
```jsx
import { useEffect } from 'react';
import { Button } from '@kameleoon/ui';
import {
  useAddData,
  useBrowser,
  useCutomData,
  useVisitorCode,
  Browser
} from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { getVisitorCode } = useVisitorCode();
  const { addData } = useAddData();
  const { addBrowser } = useBrowser();
  const { addCustomData } = useCutomData();

  useEffect(() => {
    const visitorCode = getVisitorCode('example.com');

    // Single data type passed
    addData(visitorCode, [addBrowser(Browser.Chrome)]);

    // Several data types passed
    addData(visitorCode, [addBrowser(Browser.Chrome), addCustomData(1, 'some custom value')]);
  }, []);

  ...
}
```

### `withAddData`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `addData()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import {
  withAddData,
  withBrowser,
  withCustomData,
  withVisitorCode,
  Browser,
  compose,
} from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { addData, addBrowser, addCustomData, getVisitorCode } = this.props;
    const visitorCode = getVisitorCode('example.com');

    // Single data type passed
    addData(visitorCode, [addBrowser(Browser.Chrome)]);

    // Several data types passed
    addData(visitorCode, [
      addBrowser(Browser.Chrome),
      addCustomData(1, 'some custom value'),
    ]);   
  }

  ...
}

export default compose(
  withAddData,
  withBrowser,
  withCustomData,
  withVisitorCode,
)(MyComponent);
```

## Flush data
Data associated with the current user via `addData()` is not immediately sent to the server. It is stored and accumulated until it is sent automatically by the `triggerExperiment()` or `trackConversion()`, or manually by the `flush()` callback function. This allows the developer to control exactly when the data is flushed to servers. For instance, if you call the `addData()` a dozen times, it would be a waste of resources to send data to the server after each `addData()` invocation. Just call `flush()` once at the end.

The `flush()` callback function is non-blocking as the server call is made asynchronously.

#### `flush()`
##### Arguments
- `visitorCode: string` - unique identifier of the user.

### `useFlush`
#### Returns
- A callback function `flush()`.

#### Example
```jsx
import { useEffect } from 'react';
import { Button } from '@kameleoon/ui';
import {
  useAddData,
  useBrowser,
  useVisitorCode,
  useFlush,
  Browser
} from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { getVisitorCode } = useVisitorCode();
  const { addData } = useAddData();
  const { addBrowser } = useBrowser();
  const { flush } = useFlush();

  useEffect(() => {
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addBrowser(Browser.Chrome)]);
    flush(visitorCode);
  }, []);

  ...
}
```

### `withFlush`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `flush()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import {
  withVisitorCode,
  withAddData,
  withBrowser,
  withFlush,
  Browser,
  compose,
} from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { addData, addBrowser, flush, getVisitorCode } = this.props;
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addBrowser(Browser.Chrome)]);
    flush(visitorCode);
  }

  ...
}

export default compose(
  withAddData,
  withBrowser,
  withFlush,
  withVisitorCode,
)(MyComponent);
```


## Browser
A callback function `addBrowser()` adds browser type.

#### `addBrowser`
##### Arguments
- `browser: Browser` - browser types: CHROME, INTERNET_EXPLORER, FIREFOX, SAFARI, OPERA, OTHER.

##### Returns
- An instance of `Browser: IBrowser`.

### `useBrowser`
#### Returns
- A callback function `addBrowser()`.

#### Example
```jsx
import { useEffect } from 'react';
import { useAddData, useBrowser, useVisitorCode, Browser } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { addData } = useAddData();
  const { getVisitorCode } = useVisitorCode();
  const { addBrowser } = useBrowser();

  useEffect(() => {
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addBrowser(Browser.Chrome)]);
  }, []);

  ...
}
```

### `withBrowser`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `addBrowser()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import {
  withVisitorCode,
  withAddData,
  withBrowser,
  Browser,
  compose,
} from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { addData, addBrowser, getVisitorCode } = this.props;
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addBrowser(Browser.Chrome)]);
  }

  ...
}

export default compose(
  withAddData,
  withBrowser,
  withVisitorCode,
)(MyComponent);
```

## PageView
A callback function `addPageView()` adds page view.

#### `addPageView`
##### Arguments
- `url: string` - URL of the page viewed.
- `title: string` - title of the page viewed.
- `referrer: number` (optional) - referrer of the page viewed.

##### Returns
- An instance of `PageView: IPageView`.
### `usePageView`
#### Returns
- A callback function `addPageView()`.

#### Example
```jsx
import { useEffect } from 'react';
import { useAddData, usePageView, useVisitorCode } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { addData } = useAddData();
  const { getVisitorCode } = useVisitorCode();
  const { addPageView } = usePageView();

  useEffect(() => {
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addPageView('example.com', 'title', 3)]);
  }, []);

  ...
}
```

### `withPageView`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `addPageView()` which recieves the following arguments and returns instance of `PageView: IPageView`.
  - `url: string` - URL of the page viewed.
  - `title: string` - title of the page viewed.
  - `referrer: number` (optional) - referrer of the page viewed.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import {
  withVisitorCode,
  withAddData,
  withPageView,
  compose,
} from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { addData, addPageView, getVisitorCode } = this.props;
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addPageView('example.com', 'title', 3)]);
  }

  ...
}

export default compose(
  withAddData,
  withPageView,
  withVisitorCode,
)(MyComponent);
```

## Conversion
A callback function `addConversion()` adds conversion.

#### `addConversion`
##### Arguments
- `goalId: number` - unique identifier of the goal.
- `revenue: number` (optional) - coversion revenue.
- `negative: boolean` (optional) - defines if the revenue is positive or negative.

##### Returns
- An instance of `Conversion: IConversion`.

### `useConversion`
#### Returns
- A callback function `addConversion()`.

#### Example
```jsx
import { useEffect } from 'react';
import { useAddData, useConversion, useVisitorCode } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { addData } = useAddData();
  const { getVisitorCode } = useVisitorCode();
  const { addConversion } = useConversion();

  useEffect(() => {
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addConversion(32, 10, false)]);
  }, []);

  ...
}
```

### `withConversion`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `addConversion()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import {
  withVisitorCode,
  withAddData,
  withConversion,
  compose,
} from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { addData, addConversion, getVisitorCode } = this.props;
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addConversion(32, 10, false)]);
  }

  ...
}

export default compose(
  withAddData,
  withConversion,
  withVisitorCode,
)(MyComponent);
```

## CustomData
A callback function `addCustomData()` adds custom data.

##### Please Note:
_The index (ID) of the custom data is available on our Back-Office, in the Custom data configuration page. Be careful: this index starts at **0**, so the first custom data you create for a given site will have the ID **0**, **not 1**._

#### `addCustomData`
##### Arguments
- `index: number` - index or unique identifier of the custom data to be stored.
- `value: string` - value of the custom data to be stored.

##### Returns
- An instance of `CustomData: ICustomData`.

### `useCustomData`
#### Returns
- A callback function `addCustomData()`.


#### Example
```jsx
import { useEffect } from 'react';
import { useAddData, useCustomData, useVisitorCode } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { addData } = useAddData();
  const { getVisitorCode } = useVisitorCode();
  const { addCustomData } = useCustomData();

  useEffect(() => {
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addCustomData(1, 'some custom value')]);
  }, []);

  ...
}
```

### `withCustomData`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `addCustomData()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import {
  withVisitorCode,
  withAddData,
  withCustomData,
  compose,
} from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { addData, addCustomData, getVisitorCode } = this.props;
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addCustomData(1, 'some custom value')]);
  }

  ...
}

export default compose(
  withAddData,
  withCustomData,
  withVisitorCode,
)(MyComponent);
```

## Interest
A callback function `addInterest()` adds interest.

#### `addInterest`
##### Arguments
- `index: number` - index of interest.

##### Returns
- An instance of `Interest: IInterest`.

### `useInterest`
#### Returns
- A callback function `addInterest()`.

#### Example
```jsx
import { useEffect } from 'react';
import { useAddData, useInterest, useVisitorCode } from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { addData } = useAddData();
  const { getVisitorCode } = useVisitorCode();
  const { addInterest } = useInterest();

  useEffect(() => {
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addInterest(0)]);
  }, []);

  ...
}
```

### `withInterest`
#### Arguments
- `Component: React.Component` - component which will be enhanced with the prop `addInterest()`.

#### Returns
- A wrapped component with additional props as described above.

#### Example
```jsx
import {
  withVisitorCode,
  withAddData,
  withInterest,
  compose,
} from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { addData, addInterest, getVisitorCode } = this.props;
    const visitorCode = getVisitorCode('example.com');

    addData(visitorCode, [addInterest(0)]);
  }

  ...
}

export default compose(
  withAddData,
  withInterest,
  withVisitorCode,
)(MyComponent);
```

## Helpers
### `compose`
Composes single-argument high order components.

#### Arguments
- `HOCs` - high order components to be composed.

#### Returns
- A function obtained by composing the argument HOCs, which is used as single high order component wrapper.

#### Example
```jsx
import { withVisitorCode, withTriggerExperiment, compose } from '@kameleoon/react-sdk';

class MyComponent extends React.Component {
  componentDidMount() {
    const { getVisitorCode, getVariationId } = this.props;
    const visitorCode = getVisitorCode('example.com');
    const experimentId = 230243;

    const variationId = getVariationId(visitorCode, experimentId);
  }

  ...
}

export default compose(withVisitorCode, withTriggerExperiment)(MyComponent);
```