<h1 align="center">✨ Notes ✨</h1>

# 👩‍🎤 Styling

There are two ways to use emotion, and typically you use both of them in any
given application for different use cases. The first allows you to make a
component that "carries its styles with it." The second allows you to apply
styles to a component.

### Making a styled component with emotion

Here's how you make a "styled component":

```javascript
import styled from '@emotion/styled'

const Button = styled.button`
  color: turquoise;
`

// <Button>Hello</Button>
//
//     👇
//
// <button className="css-1ueegjh">Hello</button>
```

You can also use object syntax (this is my personal preference):

```javascript
const Button = styled.button({
  color: 'turquoise',
})
```

You can even accept props by passing a function and returning the styles! 📜
https://emotion.sh/docs/styled

```javascript
const Button = styled.button(props => {
  return {
    color: props.primary ? 'hotpink' : 'turquoise',
  }
})

// or with the string form:

const Button = styled.button`
  color: ${props => (props.primary ? 'hotpink' : 'turquoise')};
`
```

### Using emotion's css prop

The styled component is only really useful for when you need to reuse a
component. For one-off styles, it's less useful. You inevitably end up creating
components with meaningless names like "Wrapper" or "Container".

Much more often I find it's nice to write one-off styles as props directly on
the element I'm rendering. Emotion does this using a special prop and a custom
JSX function (similar to `React.createElement`). You can learn more about how
this works from emotion's docs, but for this exercise, all you need to know is
to make it work, you simply add this to the top of the file where you want to
use the `css` prop:

```javascript
/** @jsx jsx */
/** @jsxFrag React.Fragment */
import {jsx} from '@emotion/core'
import React from 'react'
```

With that, you're ready to use the CSS prop anywhere in that file:

```jsx
function SomeComponent() {
  return (
    <div
      css={{
        backgroundColor: 'hotpink',
        '&:hover': {
          color: 'lightgreen',
        },
      }}
    >
      This has a hotpink background.
    </div>
  )
}

// or with string syntax:

function SomeOtherComponent() {
  const color = 'darkgreen'

  return (
    <div
      css={css`
        background-color: hotpink;
        &:hover {
          color: ${color};
        }
      `}
    >
      This has a hotpink background.
    </div>
  )
}
```

Ultimately, this is compiled to something that looks a bit like this:

```javascript
function SomeComponent() {
  return <div className="css-bp9m3j">This has a hotpink background.</div>
}
```

📜 https://emotion.sh/docs/css-prop

# 📞 HTTP Requests

Here's a quick simple example of that API in action:

```javascript
window
  .fetch('http://example.com/movies.json')
  .then(response => {
    return response.json()
  })
  .then(data => {
    console.log(data)
  })
```

All the HTTP methods are supported as well, for example, here's how you would
POST data:

```javascript
window
  .fetch('http://example.com/movies', {
    method: 'POST',
    headers: {
      'content-type': 'application/json',
      // if auth is required. Each API may be different, but
      // the Authorization header with a token is common.
      Authorization: `Bearer ${token}`,
    },
    body: JSON.stringify(data), // body data type must match "content-type" header
  })
  .then(response => {
    return response.json()
  })
  .then(data => {
    console.log(data)
  })
```

If the request fails with an unsuccessful status code (`>= 400`), then the
`response` object's `ok` property will be false. It's common to reject the
promise in this case:

```javascript
window.fetch(url).then(async response => {
  const data = await response.json()
  if (response.ok) {
    return data
  } else {
    return Promise.reject(data)
  }
})
```

It's good practice to wrap `window.fetch` in your own function so you can set
defaults (especially handy for authentication). Integrating this kind of thing
with React involves utilizing React's `useEffect` hook for making the request
and `useState` for managing the status of the request as well as the response
data and error information. 

📜 https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch

# 🏎 Authenticated HTTP requests


Here's an example of how to make an authenticated request:

```javascript
window.fetch('http://example.com/pets', {
  headers: {
    Authorization: `Bearer ${token}`,
  },
})
```

That token can really be anything that uniquely identifies the user, but a
common standard is to use a JSON Web Token (JWT). 📜 https://jwt.io

It's common to store this token in `localStorage`:

```javascript
const headers = {}
const token = window.localStorage.getItem('token')
if (token) {
  headers.Authorization = `Bearer ${token}`
}
window.fetch('http://example.com/pets', {headers})
```

In a React application you manage user authenticated state the same way you
manage any state: `useState` + `useEffect` (for making the request). When the
user providers a username and password, you make a request and if the request is
successful, the server will return the token to you which you can store in
`localStorage` for making additional requests. 

The easiest way to manage displaying the right content to the user based on
whether they've logged in, is to split your app into two parts: Authenticated,
and Unauthenticated. Then you choose which to render based on whether you have
the user's information.

📜 Learn more about this:
https://kentcdodds.com/blog/authentication-in-react-applications

# 🧹 Routing 

The de-facto standard library for routing React applications is
[React Router](https://reacttraining.com/react-router). It's terrific.

📜 **NOTE: We're using version 6 of the router which should be released soon.**

Most of the docs still apply:

https://reacttraining.com/react-router/web/guides/quick-start

but there are some differences, so as you're looking around at documentation,
reference this first:

https://github.com/ReactTraining/react-router/blob/v6.0.0-alpha.2/docs/installation/getting-started.md
https://github.com/ReactTraining/react-router/tree/v6.0.0-alpha.2/docs

The idea behind routing on the web is you have some API that informs you of
changes to the URL, then you react (no pun intended) to those changes by
rendering the correct user interface based on that URL route. 

In addition, you can change the URL when the user performs an action (like clicking a link or
submitting a form). This all happens client-side and does not reload the browser.

# ⚙️ Cache Management

State can be lumped into two buckets:

1. UI state: Modal is open, item is highlighted, etc.
2. Server cache: User data, tweets, contacts, etc.

We can drastically simplify our UI state management if we split out the server
cache into something separate.

A fantastic solution for managing the server cache on the client is
[`react-query`](https://github.com/tannerlinsley/react-query). It is a set of
React hooks that allow you to query, cache, and mutate data on your server in a
way that's flexible to support many use cases and optimizations but opinionated
enough to provide a huge amount of value.

Here are a few examples of how you can use react-query that are relevant for our
exercise:

```javascript
function readTweet(queryKey, {tweetId}) {
  return tweetClient.read(tweetId).then(data => data.tweet)
}

function App({tweetId}) {
  const result = useQuery({
    queryKey: ['tweet', {tweetId}],
    queryFn: readTweet,
  })
  // result has several properties, here are a few relevant ones:
  //   status
  //   data
  //   error
  //   isLoading
  //   isError
  //   isSuccess

  const [removeTweet, state] = useMutation(() => tweetClient.remove(tweetId))
  // call removeTweet when you want to execute the mutation callback
  // state has several properties, here are a few relevant ones:
  //   status
  //   data
  //   error
  //   isLoading
  //   isError
  //   isSuccess
}
```

📜 here are the docs:

- `useQuery`:
  https://github.com/tannerlinsley/react-query/blob/61d0bf85a37b7369067433a68e310bc1a6c23558/README.md#usequery
- `useMutation`:
  https://github.com/tannerlinsley/react-query/blob/61d0bf85a37b7369067433a68e310bc1a6c23558/README.md#usemutation
