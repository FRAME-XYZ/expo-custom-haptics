# 🎮 expo-custom-haptics

A powerful React Native module for creating rich, customizable haptic experiences with Apple AHAP files. Originally forked from [expo-ahap](https://github.com/EvanBacon/expo-ahap), now maintained and maintained by the [Equals](https://equa.ls) team.

## ✨ Features

- Full implementation of Apple's Haptic Pattern API
- Support for complex haptic patterns and sequences
- Over-the-air (OTA) audio file integration
- iOS 13+ compatibility
- TypeScript support

## 🛠 Installation

```sh
npm install expo-custom-haptics
# or
yarn add expo-custom-haptics
```

> ⚠️ Important: This module requires a physical iOS device running iOS 13 or greater.
> Run with: `npx expo run:ios -d`

## 🎯 Quick Start

```js
import { Player } from "expo-custom-haptics";

const player = new Player({
  // Your AHAP pattern
});

player.start();

// Don't forget to clean up!
player.unregister();
```

## 🎨 Create Custom Patterns

Design your own haptic patterns:
- Use the [AHAP Designer](https://ahap.fancypixel.it/) for visual pattern creation
- Learn more about haptic design at [WWDC 2021](https://developer.apple.com/videos/play/wwdc2021/10278)

## 📱 Example

Here's a sample that creates a satisfying "brrrp!" sensation:

```js
import { StyleSheet, Text, View } from "react-native";
import { Player } from "expo-custom-haptics";

const player = new Player({
  Pattern: [
    {
      Event: {
        Time: 0.0,
        EventType: "HapticContinuous",
        EventDuration: 0.6,
        EventParameters: [
          { ParameterID: "HapticIntensity", ParameterValue: 1.0 },
          { ParameterID: "HapticSharpness", ParameterValue: 0.5 },
        ],
      },
    },
    {
      ParameterCurve: {
        ParameterID: "HapticIntensityControl",
        Time: 0.0,
        ParameterCurveControlPoints: [
          { Time: 0, ParameterValue: 0.2 },
          { Time: 0.6, ParameterValue: 0.7 },
          { Time: 0.601, ParameterValue: 1.0 },
        ],
      },
    },
    {
      ParameterCurve: {
        ParameterID: "HapticSharpnessControl",
        Time: 0.0,
        ParameterCurveControlPoints: [
          { Time: 0, ParameterValue: -0.5 },
          { Time: 0.6, ParameterValue: 0.5 },
        ],
      },
    },

    {
      Event: {
        Time: 0.601,
        EventType: "HapticTransient",
        EventParameters: [
          { ParameterID: "HapticIntensity", ParameterValue: 1.0 },
          { ParameterID: "HapticSharpness", ParameterValue: 0.7 },
        ],
      },
    },
  ],
});

export default function App() {
  return (
    <View style={styles.container}>
      <Text onPress={() => player.start()}>Feel the magic! ✨</Text>
    </View>
  );
}
```

## 🎵 Audio Integration

Integrate audio files with two methods:

### 1. OTA Audio Loading

```ts
import React from "react";
import * as FS from "expo-file-system";
import { Asset } from "expo-asset";

function useAudioInDocumentsDir(res) {
  const [audioName, setName] = React.useState<string | null>(null);

  React.useEffect(() => {
    Asset.fromModule(res)
      .downloadAsync()
      .then(async (result) => {
        const audioName = result.localUri.split("/").pop();
        // move to the ios folder
        await FS.copyAsync({
          from: result.localUri,
          to: FS.documentDirectory + audioName,
        });

        setName(audioName);
      });
  }, [res]);

  return audioName;
}
```

### 2. Binary Asset Loading
Use the [link-assets config plugin](https://github.com/evanbacon/link-assets#readme) to bundle audio directly in your app binary.

## 💪 Support

Maintained with ❤️ by the [Equals](https://equa.ls) team. If you encounter any issues or have feature requests, please open an issue on our [GitHub repository](https://github.com/FRAME-XYZ/expo-custom-haptics).

## 📄 License

ISC License

---

<p align="center">
  Powered by <a href="https://equa.ls">Equals</a> | Making mobile experiences more immersive
</p>


