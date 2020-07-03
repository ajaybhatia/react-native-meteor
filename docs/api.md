## Meteor

`import Meteor from '@ajaybhatia/react-native-meteor';`

### `Meteor.connect(url, options)`

Connect to the Meteor Server

**url**: The URL of your Meteor Server websocket. This should typically start with `ws://` (insecure, like `http://`) or `wss://` (secure, like `https://`), and have the path `/websocket`, e.g.: `wss://myapp.meteor.com/websocket`

**options**:

- autoConnect **boolean** [true] whether to establish the connection to the server upon instantiation. When false, one can manually establish the connection with the Meteor.ddp.connect method.
- autoReconnect **boolean** [true] whether to try to reconnect to the server when the socket connection closes, unless the closing was initiated by a call to the disconnect method.
- reconnectInterval **number** [10000] the interval in ms between reconnection attempts.
- AsyncStorage **object** your preferred AsyncStorage. Defaults to `'@react-native-community/async-storage'` as a peer dependency. You will likely want to use `{ AsyncStorage } from 'react-native'` if using Expo

### `Meteor.disconnect()`

Disconnect from the Meteor server

### `Meteor.call(name, [arg1, arg2...], [asyncCallback])`

Perform a call to a method

### `Meteor.subscribe(name, [arg1, arg2, arg3])`

Subscribe to a collection

### `Meteor.user()`

Returns the logged in user

### `Meteor.users`

Access the meteor users collection

### `Meteor.userId()`

Returns the userId of the logged in user

### `Meteor.status()`

Gets the current connection status. Returns an object with the following properties:

**connected**: Boolean

**status**: "connected" || "disconnected"

### `Meteor.loggingIn()`

Returns true if attempting to login

### `Meteor.loginWithPassword`

### `Meteor.logout`

### `Meteor.logoutOtherClients`

## withTracker

`import { withTracker } from '@ajaybhatia/react-native-meteor'`;

The `withTracker` component is used the same way as [`meteor/react-meteor-data`](https://guide.meteor.com/react.html#using-withTracker)

```javascript
export default withTracker(() => {
  let handle = Meteor.subscribe('mySubscription');
  let loading = !handle.ready();
  let myStuff = Stuff.find({}).fetch();

  return {
    myStuff,
  };
})(MyComponent);
```

## useTracker (Experimental)

`import { useTracker } from '@ajaybhatia/react-native-meteor'`;

The `useTracker` component is used the same way as [`meteor/react-meteor-data`](https://github.com/meteor/react-packages/tree/master/packages/react-meteor-data#usetrackerreactivefn-deps-hook)

```javascript
function Foo({ listId }) {
  const currentUser = useTracker(() => Meteor.user(), []);

  const listLoading = useTracker(() => {
    const handle = Meteor.subscribe('todoList', listId);
    return !handle.ready();
  }, [listId]);
  const tasks = useTracker(() => Tasks.find({ listId }).fetch(), [listId]);

  return (
    ...
  );
}
```

## ReactiveDict

`import { ReactiveDict } from '@ajaybhatia/react-native-meteor'`

https://atmospherejs.com/meteor/reactive-dict

## Mongo

`import { Mongo } from '@ajaybhatia/react-native-meteor';`

#### `Mongo.Collection(collectionName, options)`

_collectionName_: Name of the remote collection, or pass `null` for a client-side collection

**options**:

- [.insert(doc, callback)](http://docs.meteor.com/#/full/insert)
- [.update(id, modifier, [options], [callback])](http://docs.meteor.com/#/full/update)
- [.remove(id, callback(err, countRemoved))](http://docs.meteor.com/#/full/remove)

#### _Cursor_.observe

Mirrors Meteor's observe behavior. Accepts object with the properties `added`, `changed`, and `removed`.

## Accounts

`import { Accounts } from '@ajaybhatia/react-native-meteor';`

- [Accounts.createUser](http://docs.meteor.com/#/full/accounts_createuser)
- [Accounts.changePassword](http://docs.meteor.com/#/full/accounts_forgotpassword)
- [Accounts.forgotPassword](http://docs.meteor.com/#/full/accounts_changepassword)
- [Accounts.resetPassword](http://docs.meteor.com/#/full/accounts_resetpassword)
- [Accounts.onLogin](http://docs.meteor.com/#/full/accounts_onlogin)
- [Accounts.onLoginFailure](http://docs.meteor.com/#/full/accounts_onloginfailure)
- `Accounts._hashPassword` - SHA-256 hashes password, for use with methods that may require authentication
