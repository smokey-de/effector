---
title: useUnit
lang: en-US
---

# useUnit

Bind effector stores to solid reactivity system or, in the case of events/effects - bind to current [_scope_](/api/effector/Scope.md) to use in dom event handlers.
Only `effector-solid/scope` version works this way, `useUnit` of `effector-solid` is no-op for events and does not require `Provider` with scope.

## `useUnit(unit)` {#useUnit-unit}

### Arguments {#useUnit-unit-arguments}

1. `unit` ([_Event_](/api/effector/Event.md) or [_Effect_](/api/effector/Effect.md)): Event or effect which will be bound to current `scope`

### Returns {#useUnit-unit-returns}

(Function): Function to pass to event handlers. Will trigger given unit in current scope

### Example {#useUnit-unit-example}

```jsx
import {render} from 'solid-js/web'
import {createEvent, createStore, fork} from 'effector'
import {useUnit, Provider} from 'effector-solid/scope'

const inc = createEvent()
const $count = createStore(0).on(inc, x => x + 1)

const App = () => {
  const [count, incFn] = useUnit([$count, inc])

  return (
    <>
      <p>Count: {count}</p>
      <button onClick={() => incFn()}>increment</button>
    </>
  )
}

const scope = fork()

render(
  () => (
    <Provider value={scope}>
      <App />
    </Provider>
  ),
  document.getElementById('root'),
)
```

## `useUnit(store)` {#useUnit-store}

### Arguments {#useUnit-store-arguments}

1. `store` Effector ([_Store_](/api/effector/Store.md))

### Returns {#useUnit-store-returns}

Accessor which will subscribe to store state

### Example {#useUnit-store-example}

```js
import {createStore, createApi} from 'effector'
import {useUnit} from 'effector-solid'

const $counter = createStore(0)

const {increment, decrement} = createApi($counter, {
  increment: state => state + 1,
  decrement: state => state - 1,
})

const App = () => {
  const counter = useUnit($counter)

  return (
    <div>
      {counter}
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  )
}
```

## `useUnit(shape)`

### Arguments

1. `shape` Object or array of ([_Event_](/api/effector/Event.md) or [_Effect_](/api/effector/Effect.md) or [_Store_](/api/effector/Store.md)): Events or effects or stores as accessors which will be bound to the current `scope`

**Returns**

(Object or Array):

- if event or effect: functions with the same names or keys as argument to pass to event handlers. Will trigger given unit in current scope _Note: events or effects will be bound **only** if `useUnit` is imported from `effector-solid/scope`_
- if store: accessor signals which will subscribe to the store state

### Example

```jsx
import {render} from 'solid-js/web'
import {createStore, createEvent, fork} from 'effector'
import {useUnit, Provider} from 'effector-solid/scope'

const inc = createEvent()
const dec = createEvent()

const $count = createStore(0)
  .on(inc, x => x + 1)
  .on(dec, x => x - 1)

const App = () => {
  const count = useUnit($count)
  const handler = useUnit({inc, dec})
  // or
  const [a, b] = useUnit([inc, dec])

  return (
    <>
      <p>Count: {count}</p>
      <button onClick={() => handler.inc()}>increment</button>
      <button onClick={() => handler.dec()}>decrement</button>
    </>
  )
}

const scope = fork()

render(
  () => (
    <Provider value={scope}>
      <App />
    </Provider>
  ),
  document.getElementById('root'),
)
```
