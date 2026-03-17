# Community Search Fallback

## Source
- `https://discuss.appium.io/`

## When To Use This
Use community search only after the official references in this skill do not explain the exact symptom.

## Search Pattern
- Search the exact error string in quotes.
- Add the driver name: `uiautomator2` or `xcuitest`.
- Add the platform version and whether the target is a real device or simulator/emulator.
- Add the capability name if the issue appears right after changing one capability.

Example queries:
- `"socket hang up" uiautomator2 Android 14 emulator`
- `"Activity never started" appWaitActivity uiautomator2`
- `"WebDriverAgent" xcuitest iOS 18 real device`
- `"No matches found for Identity Binding" xcuitest`

## Common Case Playbooks
Use one of these query templates first, then run the paired local check before applying any forum suggestion.

| Case | Query Template | First Local Check |
|---|---|---|
| UiAutomator2 session drops early | `"socket hang up" uiautomator2 <android-version> <device-type>` | `adb logcat -d` and one clean session retry |
| UiAutomator2 wrong startup screen | `"Activity never started" appWaitActivity uiautomator2` | verify current activity via `adb shell dumpsys activity activities` |
| UiAutomator2 server package mismatch | `"io.appium.uiautomator2.server" "instrumentation" failed` | reinstall helper packages and retry one launch |
| UiAutomator2 no such element on visible node | `"NoSuchElementException" uiautomator2 <locator-type>` | inspect page source and confirm attribute exposure |
| UiAutomator2 install/start permission issues | `"SecurityException" uiautomator2 adb` | confirm package install state and app permissions on device |
| XCUITest WDA build/signing errors | `"WebDriverAgent" "xcodebuild" failed xcuitest` | run `appium driver doctor xcuitest` and verify signing config |
| XCUITest WDA not reachable | `"Could not proxy command to remote server" xcuitest` | capture fresh Appium logs and confirm WDA endpoint reachability |
| XCUITest app install/launch failure | `"Failed to launch app" xcuitest` | confirm app binary compatibility with target runtime/device |
| XCUITest lookup tree incomplete | `"elements not visible in source" xcuitest inspector` | rerun source snapshot and verify accessibility exposure |
| XCUITest real-device trust/dev mode issues | `"Developer Mode" xcuitest real device` | verify device unlock, trust, and Developer Mode state |

## How To Filter Results
- Prefer threads that mention the same Appium major version and the same driver.
- Prefer posts that include logs or cite official Appium docs.
- Treat advice that suggests broad resets or unrelated capability changes as low confidence until you can verify it locally.

## Accept/Reject Rule For Forum Advice
- Accept only if the suggestion maps directly to your exact error text, same driver, and similar OS/device version.
- Reject or defer suggestions that require unrelated global resets before reproducing the same failure once.
- Always re-run the smallest failing check immediately after applying a proposed fix.

## Validation Rule
Never close the issue from a forum answer alone. Reproduce the proposed fix locally and tie it back to the original failing command or locator.
