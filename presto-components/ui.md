## Handling UI Actions

We handle actions from the screen by defining types for expected actions. In our case, the possible actions could be:

* Adding a Todo item
* Deleting a Todo item
* Editing a Todo item

And now, if we look at `src/Types.purs` we can see the types defined for our screen actions:

```haskell
data MainScreenAction
  = AddTodo String
  | RemoveTodo String
  | EditTodo String
  | UpdateTodo String String
```

So our screen has to trigger an action called `AddTodo` or `RemoveTodo` with an argument that's a `String` so that it matches the possible actions we have defined. We match the actions in our `appFlow` in `src/Main.purs`

```haskell
appFlow state = do
  action <- runUI (MainScreen state)
  case action of
    AddTodo str -> addTodoFlow str
    RemoveTodo id -> appFlow (MainScreenDeleteTodo id)
    EditTodo id -> appFlow (MainScreenEditTodo id)
    UpdateTodo str id -> appFlow (MainScreenUpdateTodo str id)
```

The `case` block is where we handle what happens for every action we receive; like for `AddTodo` we call the `addTodoFlow` which handles adding a todo item to the screen whereas for `RemoveTodo` we call the same `appFlow` with a different constructor that is `MainScreenDeleteTodo`. These are matched from:

```haskell
data MainScreen = MainScreen MainScreenState

data MainScreenState
  = MainScreenInit
  | MainScreenAddTodo String String
  | MainScreenDeleteTodo String
  | MainScreenError String
```

If we look at `app.js`, specifically at the `handleScreenAction` function, we will notice the various constructors we defined as the type, are matched.

```js
const handleScreenAction = (state) => {
  switch (state.tag) {
    case "MainScreenInit": initApp();break;
    case "MainScreenAddTodo":
      appendChild(state.contents);
      break;
    case "MainScreenDeleteTodo":
      removeChild(state.contents);
      break;
    case "MainScreenError":
      console.log('This is the error: ', state.contents)
      break
    default: console.log("Invalid Tag Passed: ", state.tag);
  }
}
```



