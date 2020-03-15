# `use-local-storage-state`

> React hook with the same API as useState() but persisting the data in local storage

![](https://img.shields.io/travis/com/astoilkov/use-local-storage-state)
![](https://img.shields.io/codeclimate/coverage/astoilkov/use-local-storage-state)
![](https://img.shields.io/bundlephobia/min/use-local-storage-state)
![](https://img.shields.io/david/astoilkov/use-local-storage-state)

## Install

```shell
$ npm install use-local-storage-state
```

## Why?

Why this module even exists? There are more than a few libraries to achieve almost the same thing. I created this module in frustration with the big number of non-optimal solutions. Below are the things that this module does right. All bullets are things that some other library isn't doing. Put all together there isn't a single library that solves all these problems:

- Written in TypeScript. `useLocalStorageState()` returns absolutely the same type as React `useState()`.
- Uses `JSON.parse()` and `JSON.stringify()` to support non string values.
- Subscribes to the Window [`storage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/storage_event) event which tracks changes across browser tabs and iframe's.
- Used in a production application which is based on [Caret - Markdown Editor for Mac / Windows](https://caret.io/) and is in private beta.
- Supports creating a global hook that can be used in multiple places. See the last example in the **Usage** section.

## Usage

```typescript
import useLocalStorageState from 'use-local-storage-state'

const [todos, setTodos] = useLocalStorageState('todos', [
    'buy milk',
    'do 50 push-ups'
])
```

Complete todo list example:
```typescript
import React, { useState } from 'react'
import useLocalStorageState from 'use-local-storage-state'

export default function Todos() {
    const [query, setQuery] = useState('')
    const [todos, setTodos] = useLocalStorageState('todos', ['buy milk'])

    function onClick() {
        setTodos([...todos, query])
    }

    return (
        <>
            <input value={query} onChange={e => setQuery(e.target.value)} />
            <button onClick={onClick}>Create</button>
            {todos.map(todo => (<div>{todo}</div>))}
        </>
    )
}

```

Using the same data from the storage in multiple places:
```typescript
import { createLocalStorageStateHook } from 'use-local-storage-state'

// store.ts
export const useTodos = createLocalStorageStateHook('todos', [
    'buy milk',
    'do 50 push-ups'
])

// Todos.ts
function Todos() {
    const [todos, setTodos] = useTodos()
}

// Popup.ts
function Popup() {
    const [todos, setTodos] = useTodos()
}
```

## API

### useLocalStorageState(key, defaultValue?)

Returns the same value that React `useState()` returns.\
Look at **Usage** section for an example.

#### key

Type: `string`

The key that will be used when calling `localStorage.setItem(key)`and `localStorage.getItem(key)`.\
⚠️ Be careful with name conflicts as it is possible to access a property which is already in `localStorage` that was created from another place in the codebase or in an old version of the application.

#### defaultValue

Type: `any`
Default: `undefined`

The initial value of the data. The same as `useState(defaultValue)` property.

### createLocalStorageStateHook(key, defaultValue?)

Returns a hook to be used in multiple places.\
Look at **Usage** section for an example.

#### key

Type: `string`

The key that will be used when calling `localStorage.setItem(key)`and `localStorage.getItem(key)`.\
⚠️ Be careful with name conflicts as it is possible to access a property which is already in `localStorage` that was created from another place in the codebase or in an old version of the application.

#### defaultValue

Type: `any`
Default: `undefined`

The initial value of the data. The same as `useState(defaultValue)` property.