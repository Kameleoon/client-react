# Kameleoon React/React Native SDK

## Introduction

Kameleoon React/React Native SDK is used to work with Kameleoon Feature Flags and Experiments.
Integration of this SDK on web/mobile is easy, and its footprint is low.

This page describes the most basic configuration, for more in-depth review of all the hooks and configuration options follow [Official Kameleoon Documentation](https://developers.kameleoon.com/react-js-sdk.html)

## Contents

- [Installation](#installation)
- [Configuration](#configuration)

## Installation

- **npm** - `npm install @kameleoon/react-sdk`
- **yarn** - `yarn add @kameleoon/react-sdk`
- **pnpm** - `pnpm add @kameleoon/react-sdk`

## Configuration

1. Obtain your site code from [Kameleoon Platform](https://app.kameleoon.com/)
2. Create client and pass it to `KameleoonProvider`

```tsx
import { createClient, KameleoonProvider } from '@kameleoon/react-sdk';

const client = createClient('my_site_code');

function MyComponentWrapper(): JSX.Element {
  return (
    <KameleoonProvider client={client}>
      <MyComponent />
    </KameleoonProvider>
  );
}
```

3. Asynchronously initialize client to fetch the configuration from remote server and handle possible errors

```tsx
import { useEffect } from 'react';
import {
  useInitialize,
  useAddData,
  getBrowserVisitorCode,
  useFeatureFlagActive,
  CustomData,
} from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { initialize } = useInitialize();
  const { addData } = useAddData();
  const { getBrowserVisitorCode } = useBrowserVisitorCode();
  const { isFeatureFlagActive } = useFeatureFlagActive();
  const visitorCode = getBrowserVisitorCode('www.example.com');
  const customDataIndex = 0;

  async function waitForInitialize(): Promise<void> {
    const isReady = await initialize();

    if (isReady) {
      // -- Add targeting data
      addData(visitorCode, new CustomData(customDataIndex, 'my_data'));

      // -- Check if the feature is active for visitor
      const isMyFeatureActive = isFeatureFlagActive(
        visitorCode,
        'my_feature_key',
      );
    }
  }

  useEffect(() => {
    waitForInitialize();
  }, []);
}
```

4. `hooks` are ready to use. Note: They can be used only inside `KameleoonProvider`

```tsx
import {
  useAddData,
  useBrowserVisitorCode,
  useTriggerExperiment,
  useFeatureFlagActive,
  CustomData,
} from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { addData } = useAddData();
  // -- Hook to generate new visitorCode. For React Native use `useNativeVisitorCode` hook
  const { getBrowserVisitorCode } = useBrowserVisitorCode();
  const { triggerExperiment } = useTriggerExperiment();
  const { isFeatureFlagActive } = useFeatureFlagActive();

  const visitorCode = getBrowserVisitorCode('www.example.com');
  const experimentId = 100;
  const customDataIndex = 0;

  // -- Get variation id
  const variationId = triggerExperiment('my_visitor_code', experimentId);

  // -- Add targeting data
  addData(visitorCode, new CustomData(customDataIndex, 'my_data'));

  // -- Check if the feature is active for visitor
  const isMyFeatureActive = isFeatureFlagActive(visitorCode, 'my_feature_key');
}
```
