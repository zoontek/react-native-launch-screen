# 🚀 react-native-bootsplash

Show a splash screen during app startup. Hide it when you are ready.<br>
**For migration from the v5, check the [`MIGRATION.md` guide](./MIGRATION.md).**

[![mit licence](https://img.shields.io/dub/l/vibe-d.svg?style=for-the-badge)](https://github.com/zoontek/react-native-bootsplash/blob/main/LICENSE)
[![npm version](https://img.shields.io/npm/v/react-native-bootsplash?style=for-the-badge)](https://www.npmjs.org/package/react-native-bootsplash)
[![npm downloads](https://img.shields.io/npm/dt/react-native-bootsplash.svg?label=downloads&style=for-the-badge)](https://www.npmjs.org/package/react-native-bootsplash)
<br />
[![platform - android](https://img.shields.io/badge/platform-Android-3ddc84.svg?logo=android&style=for-the-badge)](https://www.android.com)
[![platform - ios](https://img.shields.io/badge/platform-iOS-000.svg?logo=apple&style=for-the-badge)](https://developer.apple.com/ios)

<p>
  <img width="393" src="./docs/demo_static.png" alt="Demo">
  <img width="255" src="./docs/demo.gif" alt="Demo">
</p>

## Support

This library follows the React Native [releases support policy](https://github.com/reactwg/react-native-releases/blob/main/docs/support.md).<br>
It is supporting the **latest version**, and the **two previous minor series**.

## Installation

```bash
$ npm install --save react-native-bootsplash
# --- or ---
$ yarn add react-native-bootsplash
```

_⚠️  Don't forget going into the `ios` directory to execute a `pod install`._

## Setup

### Assets generation

In order to speed up the setup, we provide a **CLI** to generate assets, create the Android Drawable XML file and the iOS Storyboard file automatically ✨.

```bash
$ npx react-native-bootsplash generate --help
# --- or ---
$ yarn react-native-bootsplash generate --help
```

The command can take multiple arguments:

```bash
Usage: react-native-bootsplash generate [options] <logo>

Generate a launch screen using a logo file path (PNG or SVG)

Options:
  --project-type <string>     Project type ("detect", "bare" or "expo") (default: "detect")
  --platforms <list>          Platforms to generate for, separated by a comma (default: "android,ios,web")
  --background <string>       Background color (in hexadecimal format) (default: "#fff")
  --logo-width <number>       Logo width at @1x (in dp - we recommend approximately ~100) (default: 100)
  --assets-output <string>    Assets output directory path (default: "assets/bootsplash")
  --flavor <string>           Android flavor build variant (where your resource directory is) (default: "main")
  --html <string>             HTML template file path (your web app entry point) (default: "public/index.html")
  --license-key <string>      License key to enable brand and dark mode assets generation
  --brand <string>            Brand file path (PNG or SVG)
  --brand-width <number>      Brand width at @1x (in dp - we recommend approximately ~80) (default: 80)
  --dark-background <string>  [dark mode] Background color (in hexadecimal format)
  --dark-logo <string>        [dark mode] Logo file path (PNG or SVG)
  --dark-brand <string>       [dark mode] Brand file path (PNG or SVG)
  -h, --help                  display help for command
```

#### 💪 Unlock the CLI full potential

In order to use the `--brand`, `--brand-width` and `--dark-*` options, you must specify a `--license-key`.

With it, the generator is able to output over **50 files** (logo and brand images generated in all pixel densities, dark mode versions, etc.), saving you (and your company!) a massive amount of time not only at creation, but also at each adjustment ⏱️

_📍 This license key grants unlimited and unrestricted usage of the generator for the buyer's purposes (meaning you can execute the assets generation as much as you want)._

<a href="https://zoontek.gumroad.com/l/bootsplash-generator">
  <img width="280" src="./docs/gumroad_button.png" alt="Gumroad button">
</a>

#### Full command usage example

```bash
# Without license key
yarn react-native-bootsplash generate svgs/light-logo.svg \
  --platforms=android,ios,web \
  --background=F5FCFF \
  --logo-width=100 \
  --assets-output=assets/bootsplash \
  --flavor=main \
  --html=public/index.html

# With license key 🔑
yarn react-native-bootsplash generate svgs/light-logo.svg \
  --platforms=android,ios,web \
  --background=F5FCFF \
  --logo-width=100 \
  --assets-output=assets/bootsplash \
  --flavor=main \
  --html=public/index.html \
  --license-key=xxxxx \
  --brand=svgs/light-brand.svg \
  --brand-width=80 \
  --dark-background=00090A \
  --dark-logo=svgs/dark-logo.svg \
  --dark-brand=svgs/dark-brand.svg
```

This tool relies on the naming conventions that are used in the `/example` project and will therefore create the following files:

```bash
# Without license key
android/app/src/main/res/drawable-mdpi/bootsplash_logo.png
android/app/src/main/res/drawable-hdpi/bootsplash_logo.png
android/app/src/main/res/drawable-xhdpi/bootsplash_logo.png
android/app/src/main/res/drawable-xxhdpi/bootsplash_logo.png
android/app/src/main/res/drawable-xxxhdpi/bootsplash_logo.png
android/app/src/main/AndroidManifest.xml
android/app/src/main/res/values/colors.xml
android/app/src/main/res/values/styles.xml

ios/YourApp/BootSplash.storyboard
ios/YourApp/Colors.xcassets/BootSplashBackground-<hash>.colorset/Contents.json
ios/YourApp/Images.xcassets/BootSplashLogo-<hash>.imageset/Contents.json
ios/YourApp/Images.xcassets/BootSplashLogo-<hash>.imageset/logo-<hash>.png
ios/YourApp/Images.xcassets/BootSplashLogo-<hash>.imageset/logo-<hash>@2x.png
ios/YourApp/Images.xcassets/BootSplashLogo-<hash>.imageset/logo-<hash>@3x.png
ios/YourApp/Info.plist
ios/YourApp.xcodeproj/project.pbxproj

public/index.html

assets/bootsplash/manifest.json
assets/bootsplash/logo.png
assets/bootsplash/logo@1,5x.png
assets/bootsplash/logo@2x.png
assets/bootsplash/logo@3x.png
assets/bootsplash/logo@4x.png

# + Over 40 files with license key 🔑 (brand images, dark mode versions…)
```

![](./docs/cli_generator.png)

### With Expo

1. Disable `expo-splash-screen` autolinking (in your `package.json`):

```json
"expo": {
  "autolinking": {
    "exclude": [
      "expo-splash-screen"
    ]
  }
}
```

2. Add the plugin in your `app.json`:

```diff
{
  "expo": {
-   "splash": {
-     "image": "./assets/splash.png",
-     "resizeMode": "contain",
-     "backgroundColor": "#ffffff"
-   },
+   "plugins": [
+     ["react-native-bootsplash", { "assetsDir": "assets/bootsplash" }]
+   ]
  }
}
```

_📌 The available plugins options are:_

```ts
type PluginOptions = {
  assetsDir?: string; // optional, default is "assets/bootsplash"
  android?: {
    parentTheme?: "TransparentStatus" | "EdgeToEdge"; // optional, default is `undefined` (`Theme.BootSplash`)
    darkContentBarsStyle?: boolean; // optional, default is `undefined`
  };
};
```

### With bare React Native

#### iOS (for react-native 0.77+)

Edit the `ios/YourApp-Bridging-Header.h` to expose Obj-c methods to Swift.

```obj-c
// …

// ⬇️ Add this import
#import "RNBootSplash.h"
```

Edit the `ios/AppDelegate.swift`

```swift
import UIKit
import React
import React_RCTAppDelegate
import ReactAppDependencyProvider
// ...

@main
class AppDelegate: RCTAppDelegate {
  // ...
  
  // ⬇️ Add this and override customize method
  override func customize(_ rootView: RCTRootView!) {
    super.customize(rootView)
    RNBootSplash.initWithStoryboard("BootSplash", rootView: rootView) // ⬅️ initialize the splash screen
  }
}

```

#### iOS (for react-native < 0.77)

Edit the `ios/YourApp/AppDelegate.mm` file:

```obj-c
#import "AppDelegate.h"
#import "RNBootSplash.h" // ⬅️ add the header import

// …

@implementation AppDelegate

// …

// ⬇️ Add this before file @end (for react-native 0.74+)
- (void)customizeRootView:(RCTRootView *)rootView {
  [super customizeRootView:rootView];
  [RNBootSplash initWithStoryboard:@"BootSplash" rootView:rootView]; // ⬅️ initialize the splash screen
}

// OR

// ⬇️ Add this before file @end (for react-native < 0.74)
- (UIView *)createRootViewWithBridge:(RCTBridge *)bridge
                          moduleName:(NSString *)moduleName
                           initProps:(NSDictionary *)initProps {
  UIView *rootView = [super createRootViewWithBridge:bridge moduleName:moduleName initProps:initProps];
  [RNBootSplash initWithStoryboard:@"BootSplash" rootView:rootView]; // ⬅️ initialize the splash screen
  return rootView;
}

@end
```

#### Android

Edit your `android/app/src/main/java/com/yourapp/MainActivity.{java,kt}` file:

```java
// Java (react-native < 0.73)
// …

// add these required imports:
import android.os.Bundle;
import com.zoontek.rnbootsplash.RNBootSplash;

public class MainActivity extends ReactActivity {

  // …

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    RNBootSplash.init(this, R.style.BootTheme); // ⬅️ initialize the splash screen
    super.onCreate(savedInstanceState); // super.onCreate(null) with react-native-screens
  }
}
```

```kotlin
// Kotlin (react-native >= 0.73)
// …

// add these required imports:
import android.os.Bundle
import com.zoontek.rnbootsplash.RNBootSplash

class MainActivity : ReactActivity() {

  // …

  override fun onCreate(savedInstanceState: Bundle?) {
    RNBootSplash.init(this, R.style.BootTheme) // ⬅️ initialize the splash screen
    super.onCreate(savedInstanceState) // super.onCreate(null) with react-native-screens
  }
}
```

## API

### hide()

Hide the splash screen (immediately, or with a fade out).

#### Method type

```ts
type hide = (config?: { fade?: boolean }) => Promise<void>;
```

#### Usage

```tsx
import { useEffect } from "react";
import { Text } from "react-native";
import BootSplash from "react-native-bootsplash";

const App = () => {
  useEffect(() => {
    const init = async () => {
      // …do multiple sync or async tasks
    };

    init().finally(async () => {
      await BootSplash.hide({ fade: true });
      console.log("BootSplash has been hidden successfully");
    });
  }, []);

  return <Text>My awesome app</Text>;
};
```

### isVisible()

Return the current visibility status of the native splash screen.

#### Method type

```ts
type isVisible = () => Promise<boolean>;
```

#### Usage

```ts
import BootSplash from "react-native-bootsplash";

BootSplash.isVisible().then((value) => console.log(value));
```

### useHideAnimation()

A hook to easily create a custom hide animation by animating all splash screen elements using `Animated`, `react-native-reanimated` or else (similar as the video on top of this documentation).

#### Method type

```ts
type useHideAnimation = (config: {
  ready?: boolean; // a boolean flag to delay the animate execution (default: true)

  // the required generated assets
  manifest: Manifest; // the manifest file is generated in your assets directory
  logo?: ImageRequireSource;
  darkLogo?: ImageRequireSource;
  brand?: ImageRequireSource;
  darkBrand?: ImageRequireSource;

  // specify if you are using translucent status / navigation bars
  // in order to avoid a shift between the native and JS splash screen
  statusBarTranslucent?: boolean;
  navigationBarTranslucent?: boolean;

  animate: () => void;
}) => {
  container: ContainerProps;
  logo: LogoProps;
  brand: BrandProps;
};
```

#### Usage

```tsx
import { useState } from "react";
import { Animated, Image } from "react-native";
import BootSplash from "react-native-bootsplash";

type Props = {
  onAnimationEnd: () => void;
};

const AnimatedBootSplash = ({ onAnimationEnd }: Props) => {
  const [opacity] = useState(() => new Animated.Value(1));

  const { container, logo /*, brand */ } = BootSplash.useHideAnimation({
    manifest: require("../assets/bootsplash/manifest.json"),

    logo: require("../assets/bootsplash/logo.png"),
    // darkLogo: require("../assets/bootsplash/dark-logo.png"),
    // brand: require("../assets/bootsplash/brand.png"),
    // darkBrand: require("../assets/bootsplash/dark-brand.png"),

    statusBarTranslucent: true,
    navigationBarTranslucent: false,

    animate: () => {
      // Perform animations and call onAnimationEnd
      Animated.timing(opacity, {
        useNativeDriver: true,
        toValue: 0,
        duration: 500,
      }).start(() => {
        onAnimationEnd();
      });
    },
  });

  return (
    <Animated.View {...container} style={[container.style, { opacity }]}>
      <Image {...logo} />
      {/* <Image {...brand} /> */}
    </Animated.View>
  );
};

const App = () => {
  const [visible, setVisible] = useState(true);

  return (
    <View style={{ flex: 1 }}>
      {/* content */}

      {visible && (
        <AnimatedBootSplash
          onAnimationEnd={() => {
            setVisible(false);
          }}
        />
      )}
    </View>
  );
};
```

**This example is simple for documentation purpose (we only animate the container).**<br>
**🤙 A more complex example is available in the [`/example` folder](./example/src/AnimatedBootSplash.tsx).**

## FAQ

### How can I have a transparent status bar, or [edge-to-edge layout](https://developer.android.com/develop/ui/views/layout/edge-to-edge)?

Edit your `values/styles.xml` file to inherit from `Theme.BootSplash.TransparentStatus` / `Theme.BootSplash.EdgeToEdge` instead of `Theme.BootSplash`:

```xml
<resources>

  <!-- … -->

  <!-- make BootTheme inherit from Theme.BootSplash.TransparentStatus / Theme.BootSplash.EdgeToEdge -->
  <style name="BootTheme" parent="Theme.EdgeToEdge">
    <!-- … -->

    <!-- optional, used to enforce the initial bars styles -->
    <!-- default is true in light mode, false in dark mode -->
    <item name="darkContentBarsStyle">true</item>
  </style>

</resources>
```

### Why are both light and dark assets inlined in my index.html?

For the sake of simplicity. Since the light and dark versions of your assets are likely identical (except for the colors), if your `index.html` file is compressed with **gzip**, the size difference will be negligible.

### How should I use it with React Navigation?

If you are using React Navigation, you can hide the splash screen once the navigation container and all children have finished mounting by using the `onReady` function.

```tsx
import { NavigationContainer } from "@react-navigation/native";
import BootSplash from "react-native-bootsplash";

const App = () => (
  <NavigationContainer
    onReady={() => {
      BootSplash.hide();
    }}
  >
    {/* content */}
  </NavigationContainer>
);
```

### How can I mock the module in my tests?

Testing code which uses this library requires some setup since we need to mock the native methods.

To add the mocks, create a file `jest/setup.js` (or any other file name) containing the following code:

```ts
jest.mock("react-native-bootsplash", () => {
  return {
    hide: jest.fn().mockResolvedValue(),
    isVisible: jest.fn().mockResolvedValue(false),
    useHideAnimation: jest.fn().mockReturnValue({
      container: {},
      logo: { source: 0 },
      brand: { source: 0 },
    }),
  };
});
```

After that, we need to add the setup file in the jest config. You can add it under [setupFiles](https://jestjs.io/docs/en/configuration.html#setupfiles-array) option in your jest config file:

```json
{
  "setupFiles": ["<rootDir>/jest/setup.js"]
}
```

## Sponsors

This module is provided **as is**, I work on it in my free time.

If you or your company uses it in a production app, consider sponsoring this project 💰. You also can contact me for **premium** enterprise support: help with issues, prioritize bugfixes, feature requests, etc.

<a href="https://github.com/sponsors/zoontek"><img align="center" alt="Sponsors list" src="https://raw.githubusercontent.com/zoontek/sponsors/main/sponsorkit/sponsors.svg"></a>
