# Focus Launcher

A minimal, distraction-free Android home screen launcher built for the OnePlus Nord CE.

## Features

| Feature | How it works |
|---|---|
| **Black & White** | `ColorMatrix.setSaturation(0)` on the root view's hardware layer. Toggled per-session, persisted across reboots. |
| **Focus Mode** | A `NotificationListenerService` cancels every incoming notification that isn't a call, alarm, or high-priority direct message. |
| **Pinned apps grid** | 4-column home grid. Go to *All Apps*, tap ★ to pin/unpin. |
| **App search** | Instant filter in the app drawer. |
| **No wallpaper** | Pure black background — zero visual noise. |

---

## Build & Install

### Requirements
- Android Studio Hedgehog (2023.1.1) or newer
- JDK 17
- Android SDK 34

### Steps

```bash
# 1. Open the project in Android Studio
#    File → Open → select the FocusLauncher folder

# 2. Let Gradle sync finish

# 3. Connect your OnePlus Nord CE via USB
#    Enable: Settings → About phone → tap Build number 7× → Developer options → USB debugging

# 4. Run
./gradlew installDebug
# or press the ▶ Run button in Android Studio
```

### Set as default launcher
After installing, press the home button. Android will prompt you to choose a launcher — select **Focus** and tap **Always**.

---

## Permissions to grant manually

### 1. Notification Access (required for Focus Mode)
```
Settings → Apps → Special app access → Notification access → Focus Launcher → Allow
```

### 2. That's it
No internet, no location, no contacts. The launcher only reads installed apps.

---

## Customisation

### Change default pinned apps
Edit `FocusPrefs.kt` → `defaultPinnedApps()`. Use the exact package name:
```kotlin
private fun defaultPinnedApps() = setOf(
    "com.whatsapp",
    "com.google.android.dialer",
    "com.google.android.gm",
    "org.mozilla.firefox"
)
```

### Change allowed apps in Focus Mode
Edit `FocusPrefs.kt` → `defaultAllowedApps()`. These apps' notifications will always come through.

### Start in color mode
Edit `FocusPrefs.kt`:
```kotlin
var isGrayscale: Boolean
    get() = prefs.getBoolean("grayscale", false)   // change true → false
```

---

## Project structure

```
app/src/main/
├── java/com/focuslauncher/
│   ├── HomeActivity.kt          ← Home screen, clock, grayscale toggle
│   ├── AppDrawerActivity.kt     ← All apps + search + pin toggle
│   ├── FocusNotificationService.kt  ← Notification blocker
│   ├── AppGridAdapter.kt        ← 4-col home grid
│   ├── AppListAdapter.kt        ← Drawer list with pin button
│   ├── FocusPrefs.kt            ← SharedPreferences wrapper
│   ├── BootReceiver.kt          ← Persists state after reboot
│   └── AppItem.kt               ← Data class
└── res/
    ├── layout/
    │   ├── activity_home.xml
    │   ├── activity_app_drawer.xml
    │   ├── item_app_grid.xml
    │   └── item_app_list.xml
    ├── values/themes.xml
    ├── drawable/btn_outline.xml
    └── xml/notification_listener.xml
```

---

## OnePlus Nord CE notes

- OxygenOS may kill background services — go to **Battery → Battery optimisation → Focus Launcher → Don't optimise**
- If the notification service stops working after a few hours, also disable **App auto-launch** restrictions for Focus Launcher in OxygenOS settings
- The grayscale applies only to the launcher itself. For a phone-wide grayscale, enable **Digital Wellbeing → Wind Down** or use accessibility colour correction (Settings → Accessibility → Colour correction → Greyscale)
