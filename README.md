# Flutter Foreground/Background Event Notifier

Flutter plugin to detect when app(not Flutter container) goes to background or foreground.

## Why

Flutter has [WidgetsBindingObserver](https://api.flutter.dev/flutter/widgets/WidgetsBindingObserver-class.html) to get notified when app changes its state from active to inactive states and back. But it actually includes the state changes of the embedding Activity/ViewController as well. So if you have a plugin that opens a new activity/view controller or in iOS if you start a FaceID prompt then WidgetsBindingObserver will report as the app is inactive/resumed.

This plugin on the other hand reports the events only at app level. Since most apps need only background/foreground events this plugin is implemented with just those events. In iOS, plugin reports `didEnterBackgroundNotification` and `willEnterForegroundNotification` notifications and in Android, plugin reports these using `androidx.lifecycle:lifecycle-process` package.

Checkout `example/` project to see the differences in action.

## Getting Started

Add to your pubpsec:

```yaml
dependencies:
  ...
  flutter_fgbg: ^0.0.1
```

For null safe version:

```yaml
dependencies:
  ...
  flutter_fgbg: ^0.1.0
```

Use the built in Widget:

```dart
FGBGNotifier(
    onEvent: (event) {
        print(event); // FGBGType.foreground or FGBGType.background
    },
    child: ...,
)
```

Or consume the event stream directly:

```dart
import 'package:flutter_fgbg/flutter_fgbg.dart';

StreamSubscription<FGBGType> subscription;

...
// in initState
subscription = FGBGEvents.stream.listen((event) {
    print(event); // FGBGType.foreground or FGBGType.background
});

// in dispose
subscription.cancel();
```