---
title: Pluggable Integrations
excerpt: ""
---

{%- capture __enable-pluggable -%}
Install the `@sentry/integrations` package and provide a new instance with your config to `integrations` option. Include the plugin after the SDK has been loaded.

For example:

```bash
import * as Sentry from '@sentry/browser';
import { ReportingObserver as ReportingObserverIntegration } from '@sentry/integrations';

Sentry.init({
  dsn: '___Public_DSN___',
  integrations: [new ReportingObserverIntegration()]
});
```

### ExtraErrorData

*Import name: `Sentry.Integrations.ExtraErrorData`*

This integration extracts all non-native attributes from the Error object and attaches them to the event as the `extra` data.

Available options:

```js
{ depth: number; // limit of how deep the object serializer should go. Anything deeper than limit will be replaced with standard Node.js REPL notation of [Object], [Array], [Function] or primitive value. Defaults to 3.
}
```

### CaptureConsole

*Import name: `Sentry.Integrations.CaptureConsole`*

This integration captures all `Console API` calls and redirects them to Sentry using the `captureMessage` call. It then retriggers to preserve default native behavior.

```js
{ levels: string[]; // an array of methods that should be captured, defaults to ['log', 'info', 'warn', 'error', 'debug', 'assert']
}
```

### Dedupe

*Import name: `Sentry.Integrations.Dedupe`*

This integration deduplicates certain events; it can be helpful if you are receiving many duplicate errors. Be aware that Sentry will only compare stack traces and fingerprints.

### Debug

*Import name: `Sentry.Integrations.Debug`*

This integration allows you to inspect the content of the processed event, that will be passed to `beforeSend` and effectively send to the Sentry SDK. It will *always* run as the last integration, no matter when it was registered.

Available options:

```js
{ debugger: boolean; // trigger DevTools debugger instead of using console.log 
  stringify: boolean; // stringify event before passing it to console.log
}
```

### RewriteFrames

*Import name: `Sentry.Integrations.RewriteFrames`*

This integration allows you to apply a transformation to each frame of the stack trace. In the streamlined scenario, it can be used to change the name of the file frame it originates from, or it can be fed with an iterated function to apply any arbitrary transformation.

On Windows machines, you have to use Unix paths and skip the volume letter in `root` option to enable. For example `C:\\Program Files\\Apache\\www` won’t work, however, `/Program Files/Apache/www` will.

Available options:

```js
{ root: string; // root path that will be appended to the basename of the current frame's url iteratee: (frame) => frame); // function that takes the frame, applies any transformation on it and returns it back
}
```

### ReportingObserver

*Import name: `Sentry.Integrations.ReportingObserver`*

This integration hooks into the ReportingObserver API and sends captured events through to Sentry. It can be configured to handle only specific issue types.

Available options:

```js
{ types: <'crash'|'deprecation'|'intervention'>[];
}
```

{%- endcapture -%}

{%- include common/integration-plugin.md 
sdk_name="JavaScript"

enable-pluggable=__enable-pluggable
root_link="javascript"
 -%}