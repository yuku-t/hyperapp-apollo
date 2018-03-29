# hyperapp-apollo

[![NPM version](http://img.shields.io/npm/v/hyperapp-apollo.svg)](https://www.npmjs.com/package/hyperapp-apollo)
[![Maintainability](https://api.codeclimate.com/v1/badges/ffd3ee558d10c5ac6a7d/maintainability)](https://codeclimate.com/github/yuku-t/hyperapp-apollo/maintainability)

Hyperapp Apollo allows you to fetch data from your GraphQL server and use it in building complex
and reactive UIs using the Hyperapp framework.

## Installation

If your project is using npm, you can install [hyperapp-apollo](https://www.npmjs.com/package/hyperapp-apollo) package by npm command:

```bash
# installing the preset package and hyperapp integration
npm install --save hyperapp-apollo apollo-client-preset graphql-tag graphql

# installing each piece independently
npm install --save hyperapp-apollo apollo-client apollo-cache-inmemory apollo-link-http graphql-tag graphql
```

### Distribution files
- **dist/index.js** - The CommonJS version of this package. (default)
- **dist/hyperapp-apollo.js**, **dist/hyperapp-apollo.min.js** - IIFE version of this package. This version exports itself to `window.HyperappApollo`.

## Usage

Add the `apollo` module to your state and actions and start your application.

```js
import { apollo } from "hyperapp-apollo"
import { ApolloClient } from "apollo-client-preset"

const state = {
  apollo: {
    ...apollo.state,
    client: new ApolloClient()
  }
}

const actions = {
  apollo: apollo.actions
}

app(
  state,
  actions,
  (state, actions) => <MyComponent />,
  document.body
)
```

To connect your GraphQL data to your Hyperapp module, use `<Query/>` component generated by `query()`:

```js
import { query } from "hyperapp-apollo"
import gql from "graphql-tag"

const Query = query(gql`
  query TodoAppQuery($userId: Int!) {
    todos(userId: $userId) {
      id
      text
    }
  }
`)

export const TodoApp = ({ userId }) => (
  <Query
    variables={{
      userId
    }}
    render={({ data, loading, refetch }) => (
      <div>
        { loading ?
          <div>loading...</div>
        :
          <div>
            <button onclick={refetch}>Refresh</button>
            <ul>
              {data && data.todos && data.todos.map(todo =>
                <li key={todo.id}>{todo.text}</li>
              )}
            </ul>
          </div>
        }
      </div>
    )}
  />
}
```
