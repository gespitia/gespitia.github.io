# Demo Integration Contract

The portfolio is the host. Each live project remains independently built and versioned.

## Transport

Live demos currently run in an isolated iframe. Cross-frame communication uses `window.postMessage`.

The transport is an implementation detail. The application contract is framework-neutral so the host can later use Web Components or Module Federation without changing application semantics.

## Message envelope

```ts
type DemoMessage =
  | { source: 'gespitia-shell'; type: 'shell.init'; language: 'en' | 'es'; theme: ThemeMode; tokens: ThemeTokens }
  | { source: 'gespitia-shell'; type: 'language.changed'; language: 'en' | 'es' }
  | { source: 'gespitia-shell'; type: 'theme.changed'; theme: ThemeMode; tokens: ThemeTokens }
  | { source: 'gespitia-demo'; type: 'demo.ready'; capabilities: string[] }
  | { source: 'gespitia-demo'; type: 'demo.exit' };
```

## Theme

`inherit` means the host sends the portfolio design tokens. `native` means the demo keeps its own visual system.

Tokens are plain serializable values. No framework runtime is shared.

## Navigation

The shell owns browser navigation. A demo requests exit with `demo.exit`; the shell decides where the user goes.

## Compatibility rule

No Angular, RxJS, React, Vite or other framework dependency is shared across demos.

Each project owns its runtime and version. Only the small integration protocol is shared conceptually.
