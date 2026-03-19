# UiAutomator2 Session Startup

## Official References
- `https://github.com/appium/appium-uiautomator2-driver/blob/master/docs/activity-startup.md`
- `https://github.com/appium/appium-uiautomator2-driver?tab=readme-ov-file#troubleshooting`

## When To Read This
- `Activity never started`
- app launches the wrong screen or splash flow
- `appWaitActivity` or `appWaitPackage` mismatches
- `socket hang up`
- the UiAutomator2 server appears to die during startup

This file is a shortcut into the official driver docs, not a replacement for them.

## Local Triage
1. Verify the package and foreground activity from the device state, not from the manifest guess.
2. Align `appium:appPackage`, `appium:appActivity`, `appium:appWaitPackage`, and `appium:appWaitActivity` with the observed startup flow.
3. If the app legitimately passes through multiple transient activities, list those wait activities explicitly before increasing `appium:appWaitDuration`.
4. If logs show `socket hang up`, instrumentation failure, or helper-package errors, treat it as a UiAutomator2 helper or `adb` transport problem first.
5. If logs show install, ABI, permission, or signing failures, stop tuning startup capabilities and fix that underlying failure first.

## Minimal Checks
```bash
adb shell dumpsys window windows
adb shell dumpsys activity activities
adb shell pm list packages
adb logcat -d
```

## One Clean Retry
If startup still dies early after the first log review, do one clean retry:

```bash
adb kill-server
adb start-server
adb uninstall io.appium.uiautomator2.server
adb uninstall io.appium.uiautomator2.server.test
```

Then re-run the same single session launch and compare fresh logs before changing anything else.
