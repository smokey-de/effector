---
title: useEvent
lang: en-US
---

# useEvent

Bind event to current fork instance to use in dom event handlers. Used **only** with ssr, in application without forks `useEvent` will do nothing

## `useEvent(unit)` {#useEvent-unit}

### Arguments {#useEvent-unit-arguments}

1. `unit` ([_Event_](/api/effector/Event.md) or [_Effect_](/api/effector/Effect.md)): Event or effect which will be bound to current `scope`

### Returns {#useEvent-unit-returns}

(Function): Function to pass to event handlers. Will trigger given unit in current scope

### Example {#useEvent-unit-example}

```js
import {createDomain} from 'effector'
import {useEvent} from 'effector-vue/ssr'

const app = createDomain()

const inc = app.createEvent()
const $count = app.createStore(0).on(inc, x => x + 1)

export default {
  setup() {
    const counter = useStore($count)
    const incFn = useEvent(inc)

    return {
      incFn,
      counter,
    }
  },
}
```
