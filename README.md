# Login Page Automation - Playwright Test Framework

A scalable and maintainable Playwright test automation framework for testing login functionality.

## 📋 Project Overview

This project automates login page testing using Playwright with TypeScript. It follows the Page Object Model (POM) design pattern with Page Object Manager, custom fixtures, and data-driven testing approach.

**Live Test Report:** [View Allure Report](https://prajwalpatel20-create.github.io/noventiq_test_login/allure/)

## 🏗️ Project Structure

```
test_login/
├── actions/                      # Reusable web action utilities
│   └── webActions.ts             # Common browser interactions (click, fill, getText, fluentWait)
├── fixtures/                     # Playwright custom fixtures
│   └── fixtures.ts               # Custom page fixture setup
├── pages/                        # Page Object Models
│   ├── basePage.ts               # Base page with common methods (navigate, getUrl, getTitle)
│   ├── LoginPage.ts              # Login page actions and inline locators
│   ├── homePage.ts               # Home/Success page actions
│   ├── pageObjectManager.ts      # Centralized page object management (lazy initialization)
│   └── locators/
│       └── homePageLocators.ts   # Externalized locators for home page
├── tests/                        # Test files
│   ├── baseTest.ts               # Extended test with PageObjectManager fixture
│   └── loginPage.spec.ts         # Login test cases (10 tests)
├── test-data/                    # Test data files
│   └── testData.json             # Data-driven test inputs (organized by page)
├── utils/                        # Utility functions
│   └── env.ts                    # Environment variable management with validation
├── .github/
│   └── workflows/
│       └── playwright.yml        # GitHub Actions CI/CD pipeline
├── .husky/                       # Git hooks
│   └── pre-commit                # Pre-commit hook (lint + format check)
├── allure-report/                # Allure HTML report (generated)
├── allure-results/               # Allure test results (generated)
├── playwright-report/            # Playwright HTML report (generated)
├── test-results/                 # Test artifacts (generated)
├── playwright.config.ts          # Playwright configuration
├── tsconfig.json                 # TypeScript configuration
├── .prettierrc                   # Prettier code formatting rules
├── .prettierignore               # Files to ignore for formatting
├── package.json                  # Project dependencies and scripts
├── .env.dev                      # Development environment variables (gitignored)
├── .env.uat                      # UAT environment variables (gitignored)
└── README.md                     # Project documentation
```

## 🚀 Prerequisites

- **Node.js:** Version 18 or higher
- **npm:** Comes with Node.js

## 📥 Installation

1. **Clone the repository:**

    ```bash
    git clone <YOUR_REPO_URL>
    cd <YOUR_REPO>
    ```

2. **Install dependencies:**

    ```bash
    npm install
    ```

3. **Install Playwright browsers:**

    ```bash
    npx playwright install
    ```

4. **Create environment file:**

    Create a `.env.dev` file in the root directory with the following variables:

    ```env
    BASE_URL=<application-base-url>
    LOGIN_URL=<login-page-url>
    TEST_USERNAME=<valid-username>
    TEST_PASSWORD=<valid-password>
    ```

    > **Note:** Environment files (`.env.*`) are gitignored for security. You must create these files locally with the appropriate credentials.

## 🧪 Running Tests

### Run all tests (default environment: dev)

```bash
npm test
```

### Run tests by environment

```bash
# Development environment
npm run test:dev

# UAT environment
npm run test:uat
```

### Run tests by tag

```bash
# Run only negative test cases
npm run test:negative

# Run only positive test cases
npm run test:positive
```

### Run tests in specific browser

```bash
# Chromium only
npm run test:chromium

# Firefox only
npm run test:firefox

# WebKit (Safari) only
npm run test:webkit
```

### Run tests in headed mode (with browser UI)

```bash
npm run test:headed
```

### Run tests in debug mode

```bash
npm run test:debug
```

### Run tests with Playwright UI

```bash
npm run test:ui
```

## 📊 Test Reports

### Allure Report (Recommended)

Generate and view the Allure report with detailed test analytics:

```bash
# Generate and open Allure report
npm run allure:report

# Or separately:
npm run allure:generate  # Generate report
npm run allure:open      # Open in browser
```

### Playwright HTML Report

```bash
npm run report
```

### Report Locations

| Report Type     | Location                    | Command                 |
| --------------- | --------------------------- | ----------------------- |
| Allure Report   | `allure-report/`            | `npm run allure:report` |
| Playwright HTML | `playwright-report/`        | `npm run report`        |
| JSON Results    | `test-results/results.json` | Auto-generated          |
| Screenshots     | `test-results/`             | On failure              |
| Videos          | `test-results/`             | On failure              |

## 📝 Test Cases

This project includes **10 functional test cases** (5 negative + 5 positive):

### Negative Test Cases (`@negative`)

| ID    | Test Case         | Description                                        |
| ----- | ----------------- | -------------------------------------------------- |
| TC-01 | Empty credentials | Error when both username and password are empty    |
| TC-02 | Invalid username  | Error when username is invalid, password empty     |
| TC-03 | Empty password    | Error when username is correct, password empty     |
| TC-04 | Empty username    | Error when username is empty, password correct     |
| TC-05 | Wrong password    | Error when username is correct, password incorrect |

### Positive Test Cases (`@positive`)

| ID    | Test Case          | Description                                   |
| ----- | ------------------ | --------------------------------------------- |
| TC-06 | Successful login   | Verify login with valid credentials           |
| TC-07 | Username in messag | Verify success message contains the username  |
| TC-08 | URL & title check  | Verify correct URL and page title after login |
| TC-09 | Success message    | Verify congratulations message after login    |
| TC-10 | Logout button      | Verify logout button is visible after login   |

## 🔧 Configuration

### Environment Variables

Credentials and URLs are stored securely in environment files (`.env.dev`, `.env.uat`) which are gitignored.

| Variable        | Description          |
| --------------- | -------------------- |
| `BASE_URL`      | Application base URL |
| `LOGIN_URL`     | Login page URL       |
| `TEST_USERNAME` | Valid username       |
| `TEST_PASSWORD` | Valid password       |

### Playwright Configuration (`playwright.config.ts`)

| Setting            | Value                     |
| ------------------ | ------------------------- |
| Test Timeout       | 30 seconds                |
| Expect Timeout     | 5 seconds                 |
| Action Timeout     | 10 seconds                |
| Navigation Timeout | 15 seconds                |
| Retries            | 1 (local), 2 (CI)         |
| Workers            | Unlimited (local), 1 (CI) |
| Browsers           | Chromium, Firefox, WebKit |
| Screenshots        | On failure only           |
| Video              | Retained on failure       |
| Trace              | On first retry            |

### Reporters Configured

- **List** - Console output
- **HTML** - Playwright HTML report
- **JSON** - Machine-readable results
- **Allure** - Rich analytics dashboard

## 🏛️ Framework Design

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Test Layer                           │
│              (loginPage.spec.ts + baseTest.ts)              │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      Fixtures Layer                         │
│                       (fixtures.ts)                         │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                  Page Object Manager                        │
│            (pageObjectManager.ts - Lazy Loading)            │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│    LoginPage    │ │    HomePage     │ │    BasePage     │
│ (inline locators)│ │(external locators)│ │ (common methods)│
└─────────────────┘ └─────────────────┘ └─────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      WebActions                             │
│    (Reusable browser interactions with fluent waits)        │
└─────────────────────────────────────────────────────────────┘
```

### Key Features

| Feature                | Description                                     |
| ---------------------- | ----------------------------------------------- |
| ✅ TypeScript          | Type safety and better IDE support              |
| ✅ Page Object Model   | Maintainable and reusable page classes          |
| ✅ Page Object Manager | Centralized lazy initialization of pages        |
| ✅ Custom Fixtures     | Dependency injection for clean test setup       |
| ✅ Data-Driven Testing | JSON-based test data management                 |
| ✅ Environment Config  | Support for dev/uat environments                |
| ✅ Secure Credentials  | Environment variables (gitignored)              |
| ✅ WebActions Utility  | Reusable browser interactions with fluent waits |
| ✅ Test Tagging        | `@positive`, `@negative` for selective runs     |
| ✅ Soft Assertions     | Multiple validations in single test             |
| ✅ Cross-Browser       | Chromium, Firefox, WebKit                       |
| ✅ Parallel Execution  | Faster test runs                                |
| ✅ Allure Reports      | Rich test analytics and history                 |
| ✅ CI/CD Integration   | GitHub Actions with auto-deploy                 |
| ✅ Code Quality        | Husky pre-commit hooks + Prettier               |

### Locator Organization Patterns

This framework demonstrates two locator organization approaches:

**1. Inline Locators (LoginPage.ts):**

```typescript
export class LoginPage extends BasePage {
    readonly usernameInput = '#username';
    readonly passwordInput = '#password';
}
```

**2. External Locators (homePageLocators.ts):**

```typescript
export const HomePageLocators = {
    postTitle: { id: '.post-title', type: 'css', text: 'Logged In Successfully' },
} as const;
```

## 🚀 CI/CD Pipeline

### GitHub Actions Workflow

The project includes a comprehensive GitHub Actions workflow that:

1. **Triggers on:** Push to main/master, Pull requests, Manual dispatch
2. **Manual Run Options:**
    - Test tag: `all`, `@positive`, `@negative`
    - Browser: `chromium`, `firefox`, `webkit`, `all`
    - Environment: `dev`, `uat`
3. **Generates:** Allure report deployed to GitHub Pages
4. **Report URL:** < DEPLOYED_REPORT_URL >

### GitHub Secrets Required

| Secret          | Description          |
| --------------- | -------------------- |
| `BASE_URL`      | Application base URL |
| `LOGIN_URL`     | Login page URL       |
| `TEST_USERNAME` | Valid test username  |
| `TEST_PASSWORD` | Valid test password  |

## 🔍 Code Quality

### Pre-commit Hooks (Husky)

The following checks run before each commit:

1. **TypeScript Lint:** `npm run lint` - Type checking with `tsc --noEmit`
2. **Format Check:** `npm run format:check` - Prettier formatting validation

### Code Formatting (Prettier)

```bash
# Format all files
npm run format

# Check formatting without changes
npm run format:check
```

**Prettier Configuration (`.prettierrc`):**

- Single quotes
- 4-space indentation
- Trailing commas (ES5)
- 120 character line width

## 📚 Available Scripts

| Script                    | Description                     |
| ------------------------- | ------------------------------- |
| `npm test`                | Run all tests                   |
| `npm run test:dev`        | Run tests in dev environment    |
| `npm run test:uat`        | Run tests in UAT environment    |
| `npm run test:negative`   | Run only @negative tagged tests |
| `npm run test:positive`   | Run only @positive tagged tests |
| `npm run test:chromium`   | Run tests in Chromium only      |
| `npm run test:firefox`    | Run tests in Firefox only       |
| `npm run test:webkit`     | Run tests in WebKit only        |
| `npm run test:headed`     | Run tests with visible browser  |
| `npm run test:debug`      | Run tests in debug mode         |
| `npm run test:ui`         | Open Playwright UI mode         |
| `npm run report`          | Open Playwright HTML report     |
| `npm run allure:generate` | Generate Allure report          |
| `npm run allure:open`     | Open Allure report              |
| `npm run allure:report`   | Generate and open Allure report |
| `npm run lint`            | Run TypeScript type checking    |
| `npm run format`          | Format code with Prettier       |
| `npm run format:check`    | Check code formatting           |

## 👤 Author

**Prajwal Patel**

- GitHub: [@prajwalpatel20-create](https://github.com/prajwalpatel20-create)

## 📄 License

This project is licensed under the ISC License.
