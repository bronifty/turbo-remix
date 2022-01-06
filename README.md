# TurboRepo with Remix

```
npx create-turbo@latest ./turborepo
cd turborepo
npx create-remix@latest ./apps/remix
```

- switch the default next app port from 3000 to another port like 3002 so that when they all run together remix can run on port 3000 (idk how to manually set remix port)

- add version and ui dependency to remix app package.json

```json
/* ./apps/remix/package.json */
{
  "private": true,
  "name": "remix",
  "version": "0.0.0",
...
  "dependencies": {
    "@remix-run/react": "^1.1.1",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "remix": "^1.1.1",
    "@remix-run/serve": "^1.1.1",
    "@remix-run/vercel": "^1.1.1",
    "ui": "*"
  },
...
}
```

- supply tsup dependency and scripts to ui package’s package.json ( can swap out whole file)

```json
/* ./packages/ui/package.json */
{
  "name": "ui",
  "version": "0.0.0",
  "private": true,
  "main": "./dist/index.js",
  "module": "./dist/index.mjs",
  "types": "./dist/index.d.ts",
  "files": ["dist/**"],
  "scripts": {
    "build": "tsup index.tsx --format esm,cjs --dts --external react",
    "dev": "tsup index.tsx --format esm,cjs --watch --dts --external react",
    "lint": "TIMING=1 eslint src --fix",
    "clean": "rm -rf .turbo && rm -rf node_modules && rm -rf dist"
  },
  "devDependencies": {
    "@types/react": "^17.0.37",
    "@types/react-dom": "^17.0.11",
    "tsconfig": "*",
    "config": "*",
    "typescript": "^4.5.3",
    "tsup": "^5.10.1"
  }
}
```

```
yarn
turbo run dev
```

- import ui components into remix

```jsx
/* ./apps/remix/app/routes/index.jsx */

import { Button, CounterButton } from 'ui';
export default function Index() {
  return (
    <div>
      <Button />
      <CounterButton />
    </div>
  );
}
```

- CounterButton

```jsx
/* ./packages/ui/Counterbutton.tsx */

import * as React from 'react';

export const CounterButton = () => {
  const [count, setCount] = React.useState(0);
  return (
    <div
      style={{
        background: `rgba(255,255,255,.05)`,
        borderRadius: `8px`,
        padding: 16,
      }}
    >
      <p>
        This is component iadasdfsdfdffsfsds from <code>ui</code>
      </p>
      <p>
        <button type='button' onClick={() => setCount((c) => c + 1)}>
          count {count}
        </button>
      </p>
    </div>
  );
};
```

- Add the export from index.tsx

```js
/* ./packages/ui/index.tsx */

import * as React from 'react';
export * from './Button';
export * from './CounterButton';
```

.
