# Meteor React Native

A set of packages allowing you to connect your React Native app to your Meteor server, and take advantage of Meteor-specific features like accounts, reactive data trackers, etc.

[Full API Documentation](/docs/api.md)

If you're new to React Native, you can view a guide to using React Native with Meteor on the [Official Meteor Guide](https://guide.meteor.com/react-native.html)

# Installation

1. `npm install --save @ajaybhatia/react-native-meteor`
2. Confirm you have peer dependencty `@react-native-community/netinfo` installed
3. Confirm you have `@react-native-community/async-storage@>=1.8.1` installed. If you are using Expo, or otherwise cannot use `@react-native-community/async-storage`, see _Custom Storage Adapter_ below.

### A note on AsyncStorage

This package uses `@react-native-community/async-storage` by default. This may cause issues if you are using certain React Native versions, or if you are using Expo. To use a custom AsyncStorage implementation, pass it as an option in `Meteor.connect`:

```javascript
import { AsyncStorage } from 'react-native';

// ...

Meteor.connect('wss://myapp.meteor.com/websocket', { AsyncStorage });
```

If you are using the `AsyncStorage` API yourself, its important that you use the same version that MeteorRN is using, or issues could be caused due to the conflicting versions. Make sure you are using the same AsyncStorage you pass into Meteor (or `@react-native-community/async-storage` if you aren't passing anything), or you can use [MeteorRN's package interface](#package-interface).

# Basic Usage

```javascript
import Meteor, { Mongo, withTracker } from '@ajaybhatia/react-native-meteor';

let MyCol = new Mongo.Collection('mycol');

Meteor.connect('wss://myapp.meteor.com/websocket'); // Note the /websocket after your URL

class App extends React.Component {
  render() {
    let { myThing } = this.props;

    return (
      <View>
        <Text>Here is the thing: {myThing.name}</Text>
      </View>
    );
  }
}

let AppContainer = withTracker(() => {
  Meteor.subscribe('myThing');
  let myThing = MyCol.findOne();

  return {
    myThing,
  };
})(App);

export default AppContainer;
```

# Companion Packages

There are a few official companion packages available to add new features. Some packages provide a polyfill for Atmosphere packages, others simplify your app's integration with Native features (like local data storage).

Beta Packages:

- [`@jaybhatia/ndev-mfa`](/companion-packages/ndev-mfa): Package that allows your RN app to work with `meteor/ndev:mfa`
- [`@jaybhatia/local`](/companion-packages/local): Package for storing of data locally that works seamlessly with MeteorRN by injecting data into a local minimongo collection

Planned/Upcoming Packages:

- `@jaybhatia/queued-calls`: Package that allows you to queue Meteor calls that will be performed when internet/server is available

If you have an idea for a companion package, please open an issue. If you would like to publish your own companion package, we recommend a package name with the prefix `mrn-`.

# Compatibility

For React Native >=0.60.0 use this package

For React Native <0.60.0 use [react-native-meteor](https://github.com/inProgress-team/react-native-meteor).

**Migrating from `react-native-meteor`:**

- cursoredFind is no longer an option. All .find() calls will return cursors (to match Meteor)
- `MeteorListView` & `MeteorComplexListView` have been removed
- `CollectionFS` has been removed
- `createContainer` has been removed
- Mixins (`connectMeteor`) have been removed
- `composeWithTracker` has been removed

# Package Interface

To ensure that MeteorRN companion packages use the same versions of external packages like AsyncStorage as the core, `@ajaybhatia/react-native-meteor` provides a package interface, where companion packages can access certain packages. Currently package interface returns an object with the following properties:

- AsyncStorage

### Usage

```
import Meteor from '@ajaybhatia/react-native-meteor';

const {AsyncStorage} = Meteor.packageInterface();
```
