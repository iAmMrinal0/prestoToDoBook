## Handling API Errors

Whenever we have an input or output operation, there are chances that we can encounter an error, similarly, here we have a network call and this could result in an error. So let's see how we can handle errors when an API fails or doesn't go as we expected.

```haskell
resp <- callAPI (Headers []) TimeReq
case resp of
  Left err -> appFlow MainScreenInit
  Right (TimeResp scc) -> appFlow (MainScreenAddToDo str scc)
```

We saw this code in the previous chapter and only saw how the success is handled. In our case matches, there's a `Left` and a `Right`. `Right` is our success and `Left` is our error. Currently, in our example, we are calling the `appFlow` with the `MainScreenInit` constructor meaning we are just ignoring errors.

