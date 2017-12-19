## Managing Application Storage

There's two types of storage:

* State storage
* Local Storage

### State Storage

This is when you need something to store temporarily and don't want to persist it whenever your app reloads; meaning every time you reload your app you lose the state and you start afresh. Presto provides read and write methods to handle state storage:

* `getS`: Gets some string from the state with the key you pass
* `setS`: Sets a value in the state with the key you pass



