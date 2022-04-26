# cordova-plugin-wefitter-ios

Cordova plugin for integrating WeFitter and HealthKit into your app.


##IOS
### Info.plist (Añadir en la aplicación que importa el plugin).

| Parent                              | String                                                                         | Description                                                                                                     |
| ----------------------------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| `NSHealthShareUsageDescription`     | `Esto permite poder recoger tus datos biométricos de la aplicación de salud.`  | A message to the user that explains why the app requested permission to read samples from the HealthKit store.  |
| `NSHealthUpdateUsageDescription`    | `Esto permite poder recoger tus datos biométricos de la aplicación de salud.`  | A message to the user that explains why the app requested permission to save samples to the HealthKit store.    |

# Changelog

### Changed Rubén SANITAS

- Deleted Info.plist texts conflict other plugins (must be added by the application importing the plugin).
- Updated README.md with the necessary texts to import.

## Installation

```sh
cordova plugin add https://github.com/ThunderbyteAI/cordova-plugin-wefitter-ios.git#v0.1.0
```

In Xcode for your target:

- Set the minimum deployment target to at least iOS 11.3
- Add `HealthKit` in `Signing & Capabilities` and enable `Background Delivery`
- Add `Privacy - Health Share Usage Description` in `Info` with an appropriate message
- Add `Privacy - Health Update Usage Description` in `Info` with an appropriate message
- Add an Objective-C bridging header file

Add the following to `AppDelegate.m` and change `YOUR_API_URL`:

```objective-c
#import <WeFitterLib/WeFitterLib.h>

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  // ...

  // Begin setup WeFitter
  WeFitterConfig *config = [[WeFitterConfig alloc] initWithUrl:@"YOUR_API_URL" clientId:@"" clientSecret:@"" startDate:nil];
  NSError *error;
  BOOL success = [WeFitter setupWithConfig:config error:&error];
  if (!success) {
      NSLog(@"Error setting up WeFitter: %@", error.localizedDescription);
  }
  // End setup WeFitter

  return YES;
}
```

The url should be base without `v1/ingest/` as the plugin will append this. For example: `https://api.wefitter.com/api/`.

By default data of the past 7 days will be uploaded. To override this you can pass a `startDate`.

## Usage

Add the following code inside `onDeviceReady` in `www/js/index.js` and change `YOUR_TOKEN`:

```js
var success = function (message) {
  alert(message);
};

var failure = function (message) {
  alert(message);
};

wefitterhealthkit.connect("YOUR_TOKEN", success, failure);
```

See [wefitterhealthkit.js](www/wefitterhealthkit.js) for all available functions.
