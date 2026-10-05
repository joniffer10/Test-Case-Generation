# Smoke Testing / Automation — Test Case Generation

## Purpose

Use Chrome DevTools AI Assistance to analyze an application's UI and generate **high-priority smoke test cases** for automation testing.

The goal is to quickly determine whether the application is stable enough to proceed with deeper functional testing.

---

## Steps

### 1. Open the Application in Chrome

Open the application or specific page that needs to be analyzed using **Google Chrome**.

### 2. Open Chrome DevTools

Open Chrome DevTools:

- `F12`
- `Ctrl + Shift + I` — Windows
- `Cmd + Option + I` — macOS

### 3. Open AI Assistance

Inside Chrome DevTools:

**DevTools → AI Assistance**

Paste the following prompt:

```text
Act as a Senior QA Automation Engineer with expertise in UI testing, Playwright, Cypress, and Selenium.

Your task is to analyze the provided application screen, page, or UI elements and generate SMOKE TEST CASES only.

Instructions:

1. Inspect all visible UI elements, including:
   - Buttons
   - Links
   - Input fields
   - Dropdowns
   - Checkboxes
   - Radio buttons
   - Tables
   - Navigation menus
   - Tabs
   - Search fields
   - Forms
   - Modals/Dialogs

2. Create smoke tests that validate:
   - Page loads successfully
   - Critical UI elements are visible
   - Critical buttons are clickable
   - Navigation works
   - Key forms accept input
   - Required fields are present
   - Essential business workflows can be started
   - No obvious UI-blocking issues exist

3. DO NOT create:
   - Negative test cases
   - Edge cases
   - Boundary testing
   - Performance testing
   - Security testing
   - Detailed functional test scenarios

4. Focus only on high-priority validations that determine whether the application is stable enough for deeper testing.

5. For each smoke test case, provide:

| Test Case ID | Module | Test Scenario | Preconditions | Test Steps | Expected Result | Priority |
|--------------|--------|---------------|---------------|------------|----------------|----------|

6. Generate element locators when possible.

Use the following locator priority:

1. data-testid
2. id
3. role
4. accessible text
5. CSS/XPath as the last option

7. At the end, provide:

### Smoke Test Coverage Summary
Summarize the areas covered by the generated smoke tests.

### Critical Missing Elements
Identify important elements or workflows that cannot be verified from the available UI/context.

### Recommended Automation Priority
Rank the generated smoke tests based on automation priority.

Output everything in Markdown format.

If screenshots are provided, identify all visible elements first before generating the smoke test cases.

Assume the goal is a quick production-readiness verification and NOT exhaustive testing.
```

### 4. Select the Element as Context

In **AI Assistance**, select:

**Select element as context**

Then click the relevant page, component, or UI element that needs to be analyzed.

> For a full-page smoke test, select the highest-level element that provides enough context for the page structure.

### 5. Trigger the Prompt

Submit the prompt and allow AI Assistance to analyze the selected UI.

Review the generated test cases and verify that:

- Only **smoke tests** were generated.
- Test cases cover critical workflows.
- No unnecessary edge/negative cases were included.
- Locators are usable and stable.
- The expected results are clear.
- Priority levels are appropriate.

---

## Expected Output

The generated result should follow this structure:

| Test Case ID | Module     | Test Scenario                   | Preconditions             | Test Steps                         | Expected Result               | Priority |
| ------------ | ---------- | ------------------------------- | ------------------------- | ---------------------------------- | ----------------------------- | -------- |
| ST-001       | Login      | Verify Login page loads         | Application is accessible | Open Login page                    | Login page loads successfully | High     |
| ST-002       | Login      | Verify login fields are visible | Login page is open        | Check username and password fields | Required fields are visible   | High     |
| ST-003       | Navigation | Verify Dashboard navigation     | User is logged in         | Click Dashboard                    | Dashboard opens successfully  | High     |

---

## QA Review Before Automation

Before converting the generated cases into Playwright/Cypress/Selenium automation:

### Check the Test Case

- [ ] Is this a true smoke test?
- [ ] Is the workflow critical?
- [ ] Is the expected result measurable?
- [ ] Is the test independent?
- [ ] Is the locator stable?
- [ ] Can the test run repeatedly?
- [ ] Does the test avoid unnecessary implementation details?

### Locator Preference

Use:

```text
data-testid
    ↓
id
    ↓
role
    ↓
accessible text
    ↓
CSS
    ↓
XPath
```

Avoid relying on fragile selectors such as:

```text
nth-child()
generated CSS classes
dynamic IDs
deep DOM paths
```

---

## Automation Goal

Smoke tests should answer one primary question:

> **"Is the application stable enough for us to proceed with deeper testing?"**

Smoke testing should therefore remain **fast, focused, and high priority** rather than attempting to cover every possible behavior of the application.
