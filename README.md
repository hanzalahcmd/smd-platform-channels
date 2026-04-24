# 🔋 Battery Level — Flutter Platform Channel Demo

A minimal Flutter app that demonstrates **Platform Channels** by fetching the native device battery level from Android using Kotlin.

---

## 📱 Screenshots

| Idle State | Battery Retrieved |
|:-:|:-:|
| ![Idle screen showing the Get Battery Level button](https://github.com/hanzalahcmd/smd-platform-channels/blob/main/WhatsApp%20Image%202026-04-25%20at%2012.25.54%20AM.jpeg) | ![Screen showing the fetched battery percentage](https://github.com/hanzalahcmd/smd-platform-channels/blob/main/WhatsApp%20Image%202026-04-25%20at%2012.26.08%20AM.jpeg) |

---

## 🧠 How It Works

This project uses a [Flutter Platform Channel](https://docs.flutter.dev/platform-integration/platform-channels) to bridge Dart and native Android (Kotlin) code.

```
Flutter (Dart)  ──►  MethodChannel  ──►  Android (Kotlin)
                         │
                  'samples.flutter.dev/battery'
                         │
                  getBatteryLevel()
```

1. The Dart side calls `platform.invokeMethod('getBatteryLevel')` on the channel `samples.flutter.dev/battery`
2. `MainActivity.kt` intercepts the call via `setMethodCallHandler`
3. The native `BatteryManager` API reads the battery level and returns it as an `Int`
4. The Dart side receives the result and updates the UI

---

## 📁 Project Structure

```
lib/
└── main.dart                  # Flutter UI + MethodChannel call

android/
└── app/src/main/kotlin/
    └── com/example/batterylevel/
        └── MainActivity.kt    # Native Kotlin handler

screenshots/
├── idle.png                   # App on launch
└── battery_result.png         # After tapping the button
```

---

## 🚀 Getting Started

### Prerequisites
- Flutter SDK (`>=3.0.0`)
- Android Studio or VS Code with Flutter extension
- An Android device or emulator

### Run

```bash
git clone https://github.com/your-username/battery-level-platform-channel.git
cd battery-level-platform-channel
flutter pub get
flutter run
```

---

## 🔌 Channel Details

| Property | Value |
|---|---|
| Channel name | `samples.flutter.dev/battery` |
| Channel type | `MethodChannel` |
| Method | `getBatteryLevel` |
| Return type | `int` (battery %) |
| Error code | `UNAVAILABLE` |

---

## 🤖 Native Code (Kotlin)

```kotlin
private fun getBatteryLevel(): Int {
    return if (VERSION.SDK_INT >= VERSION_CODES.LOLLIPOP) {
        val batteryManager = getSystemService(Context.BATTERY_SERVICE) as BatteryManager
        batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY)
    } else {
        val intent = ContextWrapper(applicationContext)
            .registerReceiver(null, IntentFilter(Intent.ACTION_BATTERY_CHANGED))
        intent!!.getIntExtra(BatteryManager.EXTRA_LEVEL, -1) * 100 /
                intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1)
    }
}
```

---

## 📖 References

- [Flutter Platform Channels Docs](https://docs.flutter.dev/platform-integration/platform-channels)
- [Android BatteryManager API](https://developer.android.com/reference/android/os/BatteryManager)
