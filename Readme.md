# Includes outside of root issue

## packages/web
In the web package, the svelte.config.js has a path alias to the UI package.
```
import adapter from '@sveltejs/adapter-auto';
import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';

/** @type {import('@sveltejs/kit').Config} */
const config = {
    // Consult https://kit.svelte.dev/docs/integrations#preprocessors
    // for more information about preprocessors
    preprocess: vitePreprocess(),

    kit: {
        // adapter-auto only supports some environments, see https://kit.svelte.dev/docs/adapter-auto for a list.
        // If your environment is not supported or you settled on a specific environment, switch out the adapter.
        // See https://kit.svelte.dev/docs/adapters for more information about adapters.
        adapter: adapter(),

        alias: {
            "$ui": "../ui/src/lib",
            "$ui/*": "../ui/src/lib/*",
        }
    }
};

export default config;

```

And inside the tsconfig.json it has the ui src/lib folder in includes

```
{
  "extends": "./.svelte-kit/tsconfig.json",
  "compilerOptions": {
    "allowJs": true,
    "checkJs": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "skipLibCheck": true,
    "sourceMap": true,
    "strict": true,
    "moduleResolution": "bundler"
  },
  "include": [
    "./.svelte-kit/ambient.d.ts",
    "./.svelte-kit/non-ambient.d.ts",
    "./.svelte-kit/./types/**/$types.d.ts",
    "./vite.config.js",
    "./vite.config.ts",
    "./src/**/*.js",
    "./src/**/*.ts",
    "./src/**/*.svelte",
    "../ui/src/**/*.js",
    "../ui/src/**/*.ts",
    "../ui/src/**/*.svelte"
  ]
}

```

## The Issue

When using auto-complete in vs code the it appears for the SomeComponent.svelte and correctly uses the path alias.

When adding a new component (just add any new component in the packages/ui/src/lib) the intellisense does not pick up the new component until you restart the svelte language server