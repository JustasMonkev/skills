---
name: "appium-troubleshooting"
description: "Diagnose common Appium failures with a driver-scoped flow for UiAutomator2 or XCUITest, covering startup, driver readiness, WebDriverAgent, and element lookup issues"
metadata:
  last_modified: "Thu, 19 Mar 2026 12:00:00 GMT"

---
# appium-troubleshooting

## Goal
Help the agent narrow a single-driver Appium failure into a small set of common buckets, apply the smallest plausible fix, and re-run the failing check until the root cause is confirmed or clearly handed back to the user.

## Decision Logic
- Identify the active automation driver first (`uiautomator2` or `xcuitest`). If unknown, stop and ask for the failing session capabilities/log line that names the driver.
- If the failure is an environment or driver-install problem (`node`, `npm`, `appium`, Java, Android SDK, `Xcode`, doctor failures): run the matching setup skill for the selected driver first.
- UiAutomator2 path only:
  - session startup, wrong activity, early drop, or `socket hang up`: read [references/uiautomator2-session-startup.md](references/uiautomator2-session-startup.md).
  - locator issues: read [references/uiautomator2-locators.md](references/uiautomator2-locators.md).
- XCUITest path only:
  - WebDriverAgent startup/install/reachability, app install/launch, device/simulator state: read [references/xcuitest-troubleshooting.md](references/xcuitest-troubleshooting.md).
  - element lookup or locator issues: read [references/xcuitest-element-lookup.md](references/xcuitest-element-lookup.md) and [references/xcuitest-locators.md](references/xcuitest-locators.md).
- If the official docs do not explain the exact stack trace or symptom, use [references/community-search.md](references/community-search.md) as a fallback workflow.

## Instructions
1. **Capture the failing command and lock the driver scope**
   Record the exact error text, platform, automation driver, and relevant capabilities before changing anything. If the driver is still unclear after checking the logs or capabilities, stop and ask.

2. **Run the minimum baseline checks for that driver**
   Always collect:
   ```bash
   appium -v
   appium driver list --installed
   ```
   UiAutomator2-only:
   ```bash
   adb devices -l
   appium driver doctor uiautomator2
   ```
   XCUITest-only:
   ```bash
   xcodebuild -version
   appium driver doctor xcuitest
   ```
   If the selected-driver doctor or prerequisite check fails, switch to the matching setup skill before deeper troubleshooting.

3. **Open only the references that match the selected driver and symptom**
   Do not load both driver branches in one run. Start with the most direct reference for the observed symptom and only expand if that file does not explain the behavior.

4. **Apply one targeted change, then re-run the smallest failing check**
   Prefer narrow fixes such as capability corrections, driver cleanup, device reset, or locator updates before broader environment churn. Re-run the doctor command for prerequisite issues, the failing session launch for startup issues, or the failing locator lookup for element issues.

5. **Use official docs first, community second**
   Official references for this skill:
   - `https://appium.io/docs/en/latest/`
   - `https://github.com/appium/appium-uiautomator2-driver`
   - `https://appium.github.io/appium-xcuitest-driver/latest/`
   - `https://github.com/appium/appium-xcuitest-driver`
   Use `discuss.appium.io` only after the official references are exhausted, searching with exact error text plus driver name and platform version.

6. **Report the outcome with command evidence**
   Include the baseline checks you ran, the change you made, and the smallest reproduction or check you re-ran to confirm the result.

## Completion Criteria
Mark troubleshooting complete only when one of these is true:
- the failing check passes after a verified fix, or
- the exact blocker is isolated to something the user must do manually (for example signing, device trust, app build defects), with the relevant command output and next action captured

## Constraints
- Run commands one step at a time and re-run checks after each fix.
- Treat Appium doctor required fixes as blocking.
- Use global `appium` command mode by default unless the user explicitly asks for local `npx appium`.
- Prefer the official Appium driver docs referenced by this skill before using community answers.
- Keep troubleshooting scoped to one driver path (`uiautomator2` or `xcuitest`) per run unless the user explicitly asks for cross-driver comparison.
- Do not claim success from a theory alone; always tie the conclusion to a reproduced symptom or a passing re-check.
