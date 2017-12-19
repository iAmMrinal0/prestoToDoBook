## Managing Application Storage

There's two types of storage:

* State storage
* Local Storage

### State Storage

This is when you need something to store temporarily and don't want to persist it whenever your app reloads; meaning every time you reload your app you lose the state and you start afresh. Presto provides read and write methods to handle state storage:

* `getS`: Gets some string from the state with the key you pass
* `setS`: Sets a value in the state with the key you pass

### Local Storage

This is when you need to persist data after app restarts or reloads, like login tokens, registration tokes, or any app data. Similar to state storage, there are two methods to handle local storage too.

* `loadS`: Gets a string from local storage with the key you pass
* `saveS`: Stores a value in the local storage with the key you pass

We have used `saveS` in the `updateTodoFlow` to store the id of the todo item updated.

```haskell
Right (UpdateRes {response: result}) -> do
  saveS "data" str
  appFlow (MainScreenUpdateTodo result)
```



