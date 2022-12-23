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
