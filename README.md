# Kameleoon React SDK

## Introduction

Kameleoon React SDK is used to work with Kameleoon Feature Flags and Experiments using native JavaScript or specific React Native APIs.
The package provides a number of hooks and wrappers that can grant React oriented support for Kameleoon SDK when using it in React or React Native applications.

This page describes the most basic configuration, for more in-depth review of all the hooks and configuration options follow [Official Kameleoon Documentation](https://developers.kameleoon.com/react-js-sdk.html)

## Contents

- [Installation](#installation)
- [Configuration](#configuration)
- [Usage Example](#usage-example)

## Installation

- **npm** - `npm install @kameleoon/react-sdk`
- **yarn** - `yarn add @kameleoon/react-sdk`
- **pnpm** - `pnpm add @kameleoon/react-sdk`
- **bun** - `bun install @kameleoon/react-sdk`

## Configuration

1. Obtain your site code from [Kameleoon Platform](https://app.kameleoon.com/)
2. Instantiate SDK client and pass it to `KameleoonProvider`

```tsx
import { KameleoonProvider, createClient } from '@kameleoon/react-sdk';

const client = createClient({ siteCode: 'my_site_code' });

function MyComponentWrapper(): JSX.Element {
  return (
    <KameleoonProvider client={client}>
      <MyComponent />
    </KameleoonProvider>
  );
}
```

If used in Next.js for server-side rendering, you can set stubMode to disable SDK client initialization on the server.

```tsx
import { KameleoonProvider, createClient } from '@kameleoon/react-sdk';

const isServer = typeof window === 'undefined';

const client = createClient({ siteCode: 'my_site_code', stubMode: isServer });

function MyComponentWrapper(): JSX.Element {
  return (
    <KameleoonProvider client={client}>
      <MyComponent />
    </KameleoonProvider>
  );
}
```

## Usage Example

```tsx
import { useEffect } from 'react';
import {
  useData,
  CustomData,
  useInitialize,
  useVisitorCode,
  useFeatureFlag,
} from '@kameleoon/react-sdk';

function MyComponent(): JSX.Element {
  const { initialize } = useInitialize();
  const { addData } = useData();
  const { isFeatureFlagActive } = useFeatureFlag();
  const { getVisitorCode } = useVisitorCode();

  async function init(): Promise<void> {
    // -- Wait for the client initialization
    await initialize();

    // -- Generate a visitor code
    const visitorCode = getVisitorCode();

    // -- Add targeting data
    const customDataIndex = 0;
    addData(visitorCode, new CustomData(customDataIndex, 'my_data'));

    // -- Check if the feature is active for visitor
    const isActive = isFeatureFlagActive(visitorCode, 'my_feature_key');
  }

  useEffect(() => {
    init();
  }, []);
}
```
