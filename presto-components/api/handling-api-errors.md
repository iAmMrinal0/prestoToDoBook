## Handling API Errors

Whenever we have an input or output operation, there are chances that we can encounter an error. Similarly, here we have a network call and this could result in an error. So let's see how we can handle errors when an API fails or doesn't go as we expected.

```haskell
resp <- callAPI (Headers []) TimeReq
case resp of
  Left err -> appFlow MainScreenInit
  Left err -> appFlow (MainScreenError (show err))
  Right (TimeResp scc) -> appFlow (MainScreenAddTodo str scc)
```

We saw this code in the previous chapter and only looked at how the success response is handled. In our case matches, there's a `Left` and a `Right`, `Right` is our success and `Left` is our error. Currently, in our example, we call the `appFlow` with a different constructor `MainScreenError` and the error message \(The `show` method makes the error as a string.\)

