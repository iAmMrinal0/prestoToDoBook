## How to make API calls?

For our example, we have implemented a mock server which just returns the current time when we call the API. We need to define our request and response types to make an API call. Let's look at our types in `src/Types.purs`

```haskell
data TimeReq = TimeReq
data TimeResp = TimeResp String
```

Here, our request type is just a constructor called `TimeReq` whereas our response type is `TimeResp` has a `String` with it.

#### Understanding the instance:

If you notice, after the type declarations we have an instance defined. This is the instance which describes how our API call is implemented.

```haskell
instance getTimeReq :: RestEndpoint TimeReq TimeResp where
  makeRequest _ headers = defaultMakeRequest_ GET ("http://localhost:3000") headers
  decodeResponse body = defaultDecodeResponse body
```

* Method: `GET`
* URL: `http://localhost:3000` 
* Headers: `headers`

Now we will look at how to call the API. We do that in `src/Main.purs` in the \`addTodoFlow

```haskell
resp <- callAPI (Headers []) TimeReq
```

We use a method provided by Presto which is `callAPI` which takes the required headers with the required body/data as its argument. In our case, as we don't have a body for the API, we will just pass the request type i.e. `TimeReq` . Following the API call is how we handle the response.

```haskell
case resp of
  Left err -> appFlow MainScreenInit
  Right (TimeResp scc) -> appFlow (MainScreenAddToDo str scc)
```

Currently, we are not concerned with errors so our focus is on the \`Right of the response.

```haskell
Right (TimeResp scc) -> appFlow (MainScreenAddToDo str scc)
```

We match the response to our expected type that is `TimeResp` and our expected response value is the variable `scc`. So now we send the Todo item value and the response string to the screen with a different constructor and call the `appFlow` again.

