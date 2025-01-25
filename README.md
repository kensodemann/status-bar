# Status Bar Capacitor v7 vs v6

This repository contains two branches:

- `main`: Capacitor v7
- `cap-6`: Capacitor v6

When changing branches in order to build for different versions of Capacitor, be sure to follow _all_ of these steps:

- Close Android Studio (avoids having to remember to sync Gradle later).
- `npm i`
- `npm run build`
- `npx cap open android` (you could also just be sure to "Gradle sync" in an already open AS, I just find this to be cleaner)

## Application Structure

The application is a tabs starter app with the following modifications by tab:

### Tab1Page

The status bar color is set to red. The overlay setting is left to use the default behavior on application launch, or the behavior
set in the previously visited page if navigating.

```typescript
  async ionViewWillEnter() {
    await StatusBar.setBackgroundColor({ color: '#FF0000' });
  }
```

### Tab2Page

The status bar color is set to green and is explicitly set to overlay the web view.

```typescript
  async ionViewWillEnter() {
    await StatusBar.setOverlaysWebView({ overlay: true });
    await StatusBar.setBackgroundColor({ color: '#00FF00' });
  }
```

### Tab3Page

The status bar color is set to blue and is explicitly set to _not_ overlay the web view.

```typescript
  async ionViewWillEnter() {
    await StatusBar.setOverlaysWebView({ overlay: false });
    await StatusBar.setBackgroundColor({ color: '#0000FF' });
  }
```

## Testing Devices

I used the following devices for testing:

- Pixel 2 / Android 11
- Pixel 4 / Android 13
- Pixel 7a / Android 15

## Results

### Capacitor v6

In Capacitor v6, the results are consistent across all three devices. The Capacitor v6 behavior is being used as the baseline.

- **Tab 1**
  - Color: Red
  - Overlaying Web View: false
  - Navigation Gesture Bar (or 3-Buttons): Normal
- **Tab 2**
  - Color: Green
  - Overlaying Web View: true
  - Navigation Gesture Bar (or 3-Buttons): Normal
- **Tab 2**
  - Color: Blue
  - Overlaying Web View: false
  - Navigation Gesture Bar (or 3-Buttons): Normal

### Capacitor v7

In Capacitor v7, the results are inconsistent between Android 15 and earlier versions. Differences from Capacitor v6 are noted in **bold**.

#### Android 11 and Android 13

- **Tab 1**
  - Color: Red
  - Overlaying Web View: **true**
  - Navigation Gesture Bar (or 3-Buttons): Normal
- **Tab 2**
  - Color: Green
  - Overlaying Web View: true
  - Navigation Gesture Bar (or 3-Buttons): Normal
- **Tab 2**
  - Color: Blue
  - Overlaying Web View: false
  - Navigation Gesture Bar (or 3-Buttons): Normal

Conclusions:

- **The default overlay behavior has changed** so the status bar overlays the webview.
- Setting the status bar background color works properly in these Android versions.
- The navigation gesture bar or 3-buttons display normally outside of the application at the bottom of the pages.

#### Android 15

- **Tab 1**
  - Color: **White or Dark Grey (depends on dark vs light mode)**
  - Overlaying Web View: **Probably true**
  - Navigation Gesture Bar (or 3-Buttons): **Overlays the application tab bar**
- **Tab 2**
  - Color: **White or Dark Grey (depends on dark vs light mode)**
  - Overlaying Web View: **Probably true**
  - Navigation Gesture Bar (or 3-Buttons): **Overlays the application tab bar**
- **Tab 2**
  - Color: **White or Dark Grey (depends on dark vs light mode)**
  - Overlaying Web View: **Probably true**
  - Navigation Gesture Bar (or 3-Buttons): **Overlays the application tab bar**

Conclusions:

- **The default overlay behavior appears to be always true.** This is difficult to determine as the Ionic Framework applies non-zero "safe area" padding
  to the toolbar on this device. I mark this as "probably true" because if it were to not overlay the webview, the toolbar would have too much padding.
  To best see this, use the Chrome dev tools to examine the size and padding for the `ion-toolbar` and compare between Cap v6 and Cap v7.
- **Setting the status bar background color has no effect in this Android version.**
- **The navigation gesture bar or 3-buttons partially (gesture) or fully (3-button) overlay the application's tab buttons.**
