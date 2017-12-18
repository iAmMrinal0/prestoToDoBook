## Handling UI Actions

We handle actions from the screen by defining types for expected types. In our case, the possible actions could be:

* Adding a Todo item
* Deleting a Todo item

And now, if we look at `src/Types.purs` we can see the types defined for our screen actions:

```haskell
data MainScreenAction
  = AddTodo String
  | RemoveTodo String
```

So our screen has to trigger an action called `AddTodo` or `RemoveTodo` with an argument that's a `String` so that it matches the possible actions we have defined. We match the actions in our `appFlow` in `src/Main.purs`

```haskell
appFlow state = do
  action <- runUI (MainScreen state)
  case action of
    AddTodo str -> addTodoFlow str
    RemoveTodo id -> appFlow (MainScreenDeleteTodo id)
```

The `case` block is where we handle what happens for every action we receive; like for `AddTodo` we call the `addTodoFlow` which handles adding a todo item to the screen.

