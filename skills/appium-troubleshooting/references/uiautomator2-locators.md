# UiAutomator2 Locators

## Official Reference
- `https://github.com/appium/appium-uiautomator2-driver?tab=readme-ov-file#element-location`

Use the driver docs for the full strategy list and performance tradeoffs. Keep this page for the extra triage that is easy to miss during debugging.

## Local Guidance
- Inspect the current page source before changing the selector. Verify whether the target is exposed as `content-desc`, resource id, text, or only as a visual label.
- If the app is hybrid, confirm the current context before debugging native locators.
- `-android uiautomator` is useful when ids or accessibility metadata are missing, but keep the query narrow and readable enough to debug from logs.
- Treat a working `xpath` as proof that the node exists, not as the preferred final fix. Replace it with a stronger native locator when the source exposes one.
- If the node is not present in source at all, this is usually an app-state, accessibility, or context problem rather than locator syntax.

## Validation
- Re-run the smallest failing lookup.
- Compare the returned attributes against the actual source dump.
- If an `xpath` fix works but a stronger native locator is available, prefer the native locator before closing the issue.
