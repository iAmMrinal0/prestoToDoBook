## How to make API calls?

For our example, we have implemented a mock server which just returns the current time when we call the API. We need to define our request and response types to make an API call. Let's look at our types in `src/Types.purs`

```haskell
data TimeReq = TimeReq
newtype TimeResp = TimeResp
  { code :: Int
  , status :: String
  , response :: String
  }
```

Here, our request type is just a constructor called `TimeReq` whereas our response type is `TimeResp` which is an object and the fields are:

* code - HTTP Status code
* status - Success or Failure
* response - Our actual expected response

#### Understanding the instance:

If you notice, after the type declarations we have an instance defined. This is the instance which describes how our API call is implemented.

```haskell
instance getTimeReq :: RestEndpoint TimeReq TimeResp where
  makeRequest _ headers = defaultMakeRequest_ GET "http://localhost:3000/time" headers
  decodeResponse body = defaultDecodeResponse body
```

* Method: `GET`
* URL: `http://localhost:3000/time` 
* Headers: `headers`

#### How about POST requests?

If we come a bit down, we can find another set of types defined:

```haskell
data UpdateReq = UpdateReq String String
newtype UpdateRes = UpdateRes
  { code :: Int
  , status :: String
  , response :: Array String
  }
```

And it's instance is defined as:

```haskell
instance makeUpdateReq :: RestEndpoint UpdateReq UpdateRes where
  makeRequest reqBody headers = defaultMakeRequest POST "http://localhost:3000/update" headers reqBody
  decodeResponse body = defaultDecodeResponse body
```

* Method: `POST` 
* URL: `http://localhost:3000/update` 
* Headers: `headers` 
* Request body: `reqBody`

So far we have only defined what should happen when we call the API, but we haven't performed the API call yet.

Let's look at how to do that for our first `GET` request. In `src/Main.purs`, if we look at `addTodoFlow`

```haskell
resp <- callAPI (Headers []) TimeReq
```

We use a method provided by Presto which is `callAPI` that takes the required headers with the required body/data as its argument. In our case, as we don't have a body for the API, we will just pass the request type i.e. `TimeReq`. Following is how we handle the response.

```haskell
case resp of
  Left err -> appFlow (MainScreenError (show err))
  Right (TimeResp {response: scc}) -> appFlow (MainScreenAddTodo str scc)
```

Currently, we are not concerned with errors so our focus is on the `Right` of the response.

```haskell
Right (TimeResp {response: scc}) -> appFlow (MainScreenAddTodo str scc)
```

We match the response to our expected type that is `TimeResp` and our expected response value is the variable `scc`. So now we send the Todo item value and the response string to the screen with a different constructor and call the `appFlow` again.

### POST Request with Headers

Similarly, for our other API call which is a `POST` request, the usage is defined in `updateTodoFlow`

```haskell
updateTodoFlow str id = do
  resp <- callAPI (Headers [Header "Content-Type" "application/json"]) (UpdateReq str id)
  case resp of
    Left err -> appFlow (MainScreenError (show err))
    Right (UpdateRes {response: str}) -> appFlow (MainScreenUpdateTodo str)
```

In this request, we have a request body which we need to send and hence after our type name `UpdateReq` we have our payload.

What happens when we encounter an error from the API for some reason? We will discuss this in our next chapter.

