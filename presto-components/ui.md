## Handling UI Actions

We handle actions from the screen by defining types for expected types. In our case, the possible actions could be:

* Adding a Todo item
* Deleting a Todo item

And now, if we look at `src/Types.purs` we can see the types defined for our screen actions:

```haskell
data MainScreenAction
  = AddToDo String
  | RemoveToDo String
```

So our screen has to trigger an action called `AddToDo` or `RemoveToDo` with an argument that's a `String` so that it matches the possible actions we have defined. We match the actions in our `appFlow` in `src/Main.purs`

```haskell
appFlow state = do
  action <- runUI (MainScreen state)
  case action of
    AddToDo str -> addTodoFlow str
    RemoveToDo id -> appFlow (MainScreenDeleteToDo id)
```

The `case` block is where we handle what happens for every action we receive; like for `AddToDo` we call the `addTodoFlow` which handles adding a todo item to the screen.

