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
