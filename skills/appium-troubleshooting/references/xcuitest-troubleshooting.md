# XCUITest Troubleshooting

## Official References
- `https://appium.github.io/appium-xcuitest-driver/latest/guides/troubleshooting/`
- `https://github.com/appium/appium-xcuitest-driver`

## When To Read This
- WebDriverAgent does not build, install, or stay reachable
- session startup stalls or fails on simulator or real device
- app install or launch fails on iOS
- system alerts block automation
- simulator or real-device state looks corrupted

This page is a compact entry point into the official XCUITest troubleshooting guide.

## Local Triage Map
| Symptom | Verify First | Avoid Until Confirmed |
|---|---|---|
| WDA build or signing failure | `xcode-select`, `xcodebuild -version`, `appium driver doctor xcuitest`, and real-device signing setup | changing unrelated test capabilities |
| Proxy timeout or connection reset right after session start | fresh Appium logs, WDA reachability, and real-device unlock/trust/Developer Mode state | blaming locators or app logic |
| App install or launch fails | app binary compatibility with the target runtime or device | treating it as a generic WDA failure |
| System alert blocks the test | whether the alert should be explicitly handled or automatically accepted/dismissed | enabling auto-alert handling without matching test intent |
| One simulator behaves differently from others | one clean shutdown and retry | erase/reset before confirming corruption |

## Useful Checks
```bash
xcodebuild -version
xcode-select -p
appium driver doctor xcuitest
xcrun simctl list devices
xcrun simctl list runtimes
grep -i "WebDriverAgent\|Proxying\|timed out" <appium-server-log-file>
```

## Notes
- Do not treat every iOS launch failure as a locator issue; many are WDA or device-state problems.
- If the issue only happens on one device or one simulator runtime, include that environment detail in the root-cause summary.
