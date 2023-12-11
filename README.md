# Kameleoon React/React Native SDK

## Introduction

Kameleoon React/React Native SDK is used to work with Kameleoon Feature Flags and Experiments.
Integration of this SDK on web/mobile is easy, and its footprint is low.

This page describes the most basic configuration, for more in-depth review of all the hooks and configuration options follow [Official Kameleoon Documentation](https://developers.kameleoon.com/react-js-sdk.html)

## Contents

- [Installation](#installation)
- [Configuration](#configuration)
- [Using useReactVisitorCode and useReactNativeVisitorCode (Recommended)](#using-usereactvisitorcode-recommended)
- [Legacy useBrowserVisitorCode and useNativeVisitorCode (Deprecated)](#legacy-usebrowservisitorcode-deprecated)

## Installation

- **npm** - `npm install @kameleoon/react-sdk`
- **yarn** - `yarn add @kameleoon/react-sdk`
- **pnpm** - `pnpm add @kameleoon/react-sdk`

## Configuration

1. Obtain your site code from [Kameleoon Platform](https://app.kameleoon.com/)
2. Create client and pass it to `KameleoonProvider`

```tsx
import { createClient, KameleoonProvider } from '@kameleoon/react-sdk';

const client = createClient({ siteCode: 'my_site_code' });

function MyComponentWrapper(): JSX.Element {
  return (
    <KameleoonProvider client={client}>
      <MyComponent />
    </KameleoonProvider>
  );
}
```

## Using useReactVisitorCode (Recommended)

1. Asynchronously initialize client to fetch the configuration from remote server and handle possible errors

```tsx
import { useEffect } from 'react';
import {
  useInitialize,
  useAddData,
  useReactVisitorCode,
  useFeatureFlagActive,
  CustomData,
} from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { initialize } = useInitialize();
  const { addData } = useAddData();
  // -- Hook to generate new visitorCode. For React Native use `useReactNativeVisitorCode` hook
  const { getReactVisitorCode } = useReactVisitorCode();
  const { isFeatureFlagActive } = useFeatureFlagActive();
  const customDataIndex = 0;

  async function waitForInitialize(): Promise<void> {
    const isReady = await initialize();

    if (isReady) {
      const visitorCode = getReactVisitorCode();

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

2. `hooks` are ready to use. Note: They can be used only inside `KameleoonProvider`

```tsx
import {
  useAddData,
  useReactVisitorCode,
  useFeatureFlagActive,
  CustomData,
} from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { addData } = useAddData();
  // -- Hook to generate new visitorCode. For React Native use `useReactNativeVisitorCode` hook
  const { getReactVisitorCode } = useReactVisitorCode();
  const { isFeatureFlagActive } = useFeatureFlagActive();

  const visitorCode = getReactVisitorCode();
  const customDataIndex = 0;

  // -- Add targeting data
  addData(visitorCode, new CustomData(customDataIndex, 'my_data'));

  // -- Check if the feature is active for visitor
  const isMyFeatureActive = isFeatureFlagActive(visitorCode, 'my_feature_key');
}
```

## Legacy useBrowserVisitorCode (Deprecated)

> **Note:** The `useBrowserVisitorCode` and `useNativeVisitorCode` hooks are deprecated and will be removed in a future release. Use [`useReactVisitorCode`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usereactvisitorcode) and [`useReactNativeVisitorCode`](https://developers.kameleoon.com/feature-management-and-experimentation/web-sdks/react-js-sdk#usereactnativevisitorcode) instead.

1. Asynchronously initialize client to fetch the configuration from remote server and handle possible errors

```tsx
import { useEffect } from 'react';
import {
  useInitialize,
  useAddData,
  useBrowserVisitorCode,
  useFeatureFlagActive,
  CustomData,
} from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { initialize } = useInitialize();
  const { addData } = useAddData();
  // -- Hook to generate new visitorCode. For React Native use `useNativeVisitorCode` hook
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

2. `hooks` are ready to use. Note: They can be used only inside `KameleoonProvider`

```tsx
import {
  useAddData,
  useBrowserVisitorCode,
  useFeatureFlagActive,
  CustomData,
} from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { addData } = useAddData();
  // -- Hook to generate new visitorCode. For React Native use `useNativeVisitorCode` hook
  const { getBrowserVisitorCode } = useBrowserVisitorCode();
  const { isFeatureFlagActive } = useFeatureFlagActive();

  const visitorCode = getBrowserVisitorCode('www.example.com');
  const customDataIndex = 0;

  // -- Add targeting data
  addData(visitorCode, new CustomData(customDataIndex, 'my_data'));

  // -- Check if the feature is active for visitor
  const isMyFeatureActive = isFeatureFlagActive(visitorCode, 'my_feature_key');
}
```
