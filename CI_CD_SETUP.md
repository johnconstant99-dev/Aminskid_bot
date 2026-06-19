# Branch Protection & CI/CD Setup Guide

## What's Included

This branch contains:
1. **GitHub Actions CI Workflow** (`.github/workflows/ci.yml`)
2. **Jest Configuration** (`jest.config.js`)
3. **Example Tests** (`src/__tests__/bot.test.js`)
4. **Updated package.json** with test scripts and dev dependencies

## Installation Steps

### 1. Merge This Branch
- Create a Pull Request from `add/ci-workflow` to `Main`
- Review and merge the changes

### 2. Install Dependencies Locally
```bash
npm install
```

This will install:
- `jest` - Testing framework
- `supertest` - HTTP assertion library

### 3. Run Tests Locally
```bash
# Run all tests once
npm test

# Run tests in watch mode (re-runs on file changes)
npm run test:watch
```

## Branch Protection Setup

After merging, set up branch protection rules:

1. Go to **Settings** → **Branches**
2. Click **Add branch protection rule**
3. Enter branch name: `Main`
4. Enable these settings:

### Required Settings
- ✅ **Require a pull request before merging**
  - Require approvals: `1`
  - Dismiss stale pull request approvals: ✓
  
- ✅ **Require status checks to pass before merging**
  - Search for and add these status checks:
    - `lint` (Code syntax checking)
    - `test` (Unit tests)
    - `code-quality` (Security checks)
    - `build` (Build verification)
  
- ✅ **Require branches to be up to date before merging**

- ✅ **Require conversation resolution before merging**

- ✅ **Allow force pushes** → Set to `Restrict who can force push`
  - Select: `Admins only` or `No one`

- ✅ **Allow deletions** → Set to `Restrict who can delete`
  - Select: `Admins only` or `No one`

- ✅ **Require linear history**

- ✅ **Include administrators** (if you want protection to apply to admins too)

## CI/CD Workflow Details

The workflow runs automatically on:
- Push to `Main`, `main`, or `develop`
- Pull requests to those branches

### Jobs:
1. **Lint** - Checks syntax and npm vulnerabilities
2. **Test** - Runs Jest tests with PostgreSQL
3. **Code Quality** - Detects hardcoded secrets and console statements
4. **Security** - Runs npm audit
5. **Build** - Verifies app starts correctly

### All jobs must pass before merging!

## Writing Your Tests

Replace `src/__tests__/bot.test.js` with your actual tests:

```javascript
describe('Feature Name', () => {
  test('should do something', () => {
    expect(result).toBe(expected);
  });
});
```

## Troubleshooting

- **Tests failing locally?** Check that Node 18+ is installed: `node --version`
- **DB connection errors?** Ensure PostgreSQL is running on port 5432
- **Lint errors?** Follow the error messages to fix syntax issues
- **Secrets detected?** Move sensitive data to `.env` files (already in .gitignore)

## Next Steps

1. Merge this PR
2. Write comprehensive tests for your bot
3. Monitor CI/CD runs in the **Actions** tab
4. Keep coverage above 50% (configurable in `jest.config.js`)
