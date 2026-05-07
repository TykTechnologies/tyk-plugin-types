# @tyk-technologies/tyk-plugin-types

TypeScript ambient type declarations for the Tyk gateway's JSVM plugin runtime.

These types describe the API exposed inside the gateway when authoring custom
middleware in JavaScript / TypeScript. They cover both the legacy `otto` driver
and the modern `goja` driver — both expose the same globals (`TykJS`,
`TykMakeHttpRequest`, `log`, ...) and pass the same struct shapes across the
Go↔JS boundary.

## Install

```sh
npm install --save-dev @tyk-technologies/tyk-plugin-types
```

## Use

Add a triple-slash directive at the top of your plugin entry file:

```ts
/// <reference types="@tyk-technologies/tyk-plugin-types" />

var handler = new TykJS.TykMiddleware.NewMiddleware({});

handler.NewProcessRequest(function (
  request: TykRequest,
  session: TykSession,
  config: TykConfig
): TykHandlerResult {
  request.SetHeaders["X-Hello"] = "world";
  return handler.ReturnData(request, {});
});
```

Or, if you'd rather not use a triple-slash directive, add the package to
`compilerOptions.types` in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "types": ["@tyk-technologies/tyk-plugin-types"]
  }
}
```

## What's covered

- `TykJS.TykMiddleware.NewMiddleware` — middleware constructor and the four
  return helpers (`ReturnData`, `ReturnAuthData`, `ReturnResponseData`,
  `NewProcessRequest`, `NewProcessResponse`).
- Request / response / session / config shapes (`TykRequest`, `TykResponse`,
  `TykSession`, `TykAccessDefinition`, `TykConfig`, `TykReturnOverrides`,
  `TykHandlerResult`).
- Globals registered by the gateway (`TykMakeHttpRequest`, `TykGetKeyData`,
  `TykSetKeyData`, `TykBatchRequest`, `log`, `rawlog`, `b64enc`, `b64dec`,
  `rawb64enc`, `rawb64dec`).
- Helper types for the JSON-string contracts (`TykHttpRequestSpec`,
  `TykHttpResponseRaw`).
- Manifest constants (`TykPluginDriver`, `TykMiddlewareHook`).

## Versioning

These types track the gateway's JS-side API surface. Breaking changes in the
gateway runtime that affect the surface will produce a major version bump here.

## Authoring & testing

A starter project that demonstrates how to author, build, and unit-test plugins
against these types lives at
[tyk-plugin-starter](https://github.com/TykTechnologies/tyk-plugin-starter).

## License

Apache 2.0
