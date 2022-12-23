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
