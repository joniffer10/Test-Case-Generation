# Smoke Testing / Automation — Test Case Generation

## 1. Purpose

This document defines the process for generating **smoke test cases for UI automation testing** using **Chrome DevTools AI Assistance**.

The process uses AI to quickly analyze an application's UI, identify critical elements and workflows, and generate an initial set of smoke test cases that can later be reviewed and automated.

The generated test cases should always be **reviewed and validated by a QA Engineer** before being implemented as automated tests.

---

## 2. Objective

The objective of smoke testing is to quickly determine whether an application is stable enough for deeper testing.

Smoke tests should focus on **critical and high-priority functionality**, such as:

* Application/page loading
* Critical UI elements
* Primary navigation
* Important buttons and actions
* Essential forms
* Required fields
* Critical business workflows
* Basic successful user interactions
* Obvious UI-blocking issues

Smoke testing is **not intended to provide exhaustive functional coverage**.

---

## 3. Tools

The following tools are used in this process:

* Google Chrome
* Chrome DevTools
* Chrome DevTools AI Assistance
* Playwright, Cypress, or Selenium for automation

---

## 4. Smoke Testing Workflow

```text
Open Application
       ↓
Open Chrome DevTools
       ↓
Open AI Assistance
       ↓
Select Element as Context
       ↓
Paste Smoke Test Generation Prompt
       ↓
Trigger AI Assistance
       ↓
Review Generated Test Cases
       ↓
Validate Test Cases & Locators
       ↓
Refine / Approve
       ↓
Implement Automation
```

---

## 5. Procedure

### Step 1 — Open the Application

Open the target application or page using **Google Chrome**.

Navigate to the module, feature, or page that requires smoke-test coverage.

Make sure the application is in a usable state before generating the test cases.

---

### Step 2 — Open Chrome DevTools

Open Chrome DevTools using one of the following:

| Platform | Shortcut |
| --- | --- |
| Windows/Linux | `F12` |
| Windows/Linux | `Ctrl + Shift + I` |
| macOS | `Cmd + Option + I` |

---

### Step 3 — Open AI Assistance

Inside Chrome DevTools, open:

**AI Assistance**

---

### Step 4 — Select Element as Context

Select:

**Select element as context**

Then select the relevant page, component, or UI section that should be analyzed.

#### Page-Level Testing

For a full-page smoke test, select the highest-level element that provides sufficient context for the page.

#### Module-Level Testing

For a specific module or feature, select the container representing that feature.

#### Component-Level Testing

For a specific UI component, select the component itself.

The selected context should contain enough information for AI Assistance to understand the relevant UI elements and interactions.

---

## 6. Smoke Test Case Generation Prompt

Copy and paste the following prompt into **Chrome DevTools AI Assistance**:

```text
Act as a Senior QA Automation Engineer with expertise in UI testing, Playwright, Cypress, and Selenium.

Your task is to analyze the provided application screen, page, or UI elements and generate SMOKE TEST CASES only.

Instructions:
1. Inspect all visible UI elements including:
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
   - No obvious UI blocking issues exist

3. DO NOT create:
   - Negative test cases
   - Edge cases
   - Boundary testing
   - Performance testing
   - Security testing
   - Detailed functional test scenarios

4. Focus only on high-priority validations that determine whether the application is stable enough for deeper testing.

5. For each smoke test case provide:

| Test Case ID | Module | Test Scenario | Preconditions | Test Steps | Expected Result | Priority |
|--------------|----------|---------------|---------------|------------|----------------|----------|

6. Generate element locators when possible:
   - Prefer data-testid
   - Then id
   - Then role
   - Then accessible text
   - Then CSS/XPath as last option

7. At the end provide:
   - Smoke Test Coverage Summary
   - Critical Missing Elements (if any)
   - Recommended Automation Priority

Output in Markdown format.

If screenshots are provided, identify all visible elements first before generating the smoke test cases.

Assume the goal is a quick production readiness verification and not exhaustive testing.
```

---

## 7. Trigger the Prompt

After selecting the element as context and pasting the prompt, trigger the AI Assistance request.

Allow AI Assistance to analyze the selected UI and generate the smoke test cases.

The generated result should be treated as a **draft** for QA review.

---

## 8. Generated Test Case Review

Review each generated test case before using it for automation.

The QA Engineer should verify that the test:

* Represents a critical application workflow.
* Is appropriate for smoke testing.
* Has clear preconditions.
* Contains clear and actionable steps.
* Has a measurable expected result.
* Has an appropriate priority.
* Can be executed repeatedly.
* Does not unnecessarily test detailed functionality.

Remove or move test cases that belong to other testing categories.

---

## 9. Locator Review

The prompt asks AI Assistance to generate locators using the following preference:

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

The generated locator should always be verified before being used in automation.

### Preferred

```javascript
page.getByTestId('login-button')
```

```javascript
page.getByRole('button', { name: 'Login' })
```

### Avoid Fragile Locators

```javascript
page.locator('div:nth-child(3) > button')
```

```javascript
page.locator('.css-1a2b3c')
```

```javascript
page.locator('#generated-123456')
```

Avoid selectors that depend on:

* Dynamic IDs
* Generated CSS classes
* DOM position
* `nth-child()`
* Deep DOM structures
* Implementation-specific markup

---

## 10. Expected Output

The generated test cases should follow this structure:

| Test Case ID | Module | Test Scenario | Preconditions | Test Steps | Expected Result | Priority |
| --- | --- | --- | --- | --- | --- | --- |
| ST-001 | Login | Verify Login page loads | Application is accessible | Open Login page | Login page loads successfully | High |
| ST-002 | Login | Verify login fields are visible | Login page is open | Check username and password fields | Required fields are visible | High |
| ST-003 | Navigation | Verify Dashboard navigation | User is logged in | Click Dashboard | Dashboard opens successfully | High |

The output should also include:

### Smoke Test Coverage Summary

A summary of the application areas covered by the generated smoke tests.

### Critical Missing Elements

Important UI elements or workflows that could not be verified from the available context.

### Recommended Automation Priority

A prioritized list indicating which smoke tests should be automated first.

---

## 11. QA Validation Checklist

Before approving the generated test cases:

### Test Case

* [ ] Is this a true smoke test?
* [ ] Does it cover a critical workflow?
* [ ] Are the preconditions clear?
* [ ] Are the test steps actionable?
* [ ] Is the expected result measurable?
* [ ] Is the priority appropriate?
* [ ] Is the test independent?
* [ ] Is the test repeatable?

### Automation

* [ ] Is the locator valid?
* [ ] Is the locator stable?
* [ ] Can the test run without unnecessary manual intervention?
* [ ] Is the required test data available?
* [ ] Is authentication/setup defined?
* [ ] Can the test be automated using the team's framework?

---

## 12. Scope

The smoke-test generation process intentionally focuses only on **high-priority, successful-path validation**.

The following are outside the scope of smoke testing:

* Negative testing
* Edge-case testing
* Boundary testing
* Performance testing
* Security testing
* Detailed validation rules
* Exhaustive functional testing
* Full regression testing

These scenarios should be handled by their respective test suites.

### Example

**Smoke Test**

> Verify that the user can successfully submit the login form.

**Functional Test**

> Verify that the login form accepts a valid username and password.

**Negative Test**

> Verify that login fails when an invalid password is provided.

The smoke suite should focus primarily on the first type.

---

## 13. Automation

After the generated test cases have been reviewed and approved, they can be converted into automated tests.

Supported automation frameworks may include:

* Playwright
* Cypress
* Selenium

The approved test case should serve as the **source of truth** for the automated implementation.

The automation should preserve:

* Test scenario
* Preconditions
* Test steps
* Expected result
* Priority
* Locator strategy

---

## 14. Automation Priority

Smoke tests should generally be automated based on business and application criticality.

### High Priority

Critical workflows that determine whether the application is usable.

Examples:

* Application loading
* Login
* Main navigation
* Dashboard access
* Primary transactions
* Critical form submissions

### Medium Priority

Important workflows that support the application's primary functionality.

### Low Priority

Non-critical functionality that does not significantly affect the application's basic usability.

---

## 15. Final Goal

The completed smoke test suite should answer one question:

> **Is the application stable enough for us to proceed with deeper testing?**

Smoke testing should therefore remain:

**Fast → Focused → Critical → Repeatable → Automation-friendly**

It should provide enough coverage to identify major issues early without attempting to replace functional, regression, negative, performance, or security testing.
