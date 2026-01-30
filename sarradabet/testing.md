# Testing Guide

## Introduction

This guide covers testing practices for the SarradaBet monorepo, which includes both a TypeScript/Express API (`apps/api`) and a React frontend (`apps/web`). We use **Jest** for API tests and **Vitest** for web tests, with a target of **80% code coverage** for both applications.

### Why Testing Matters

- **Confidence**: Tests give us confidence that our code works as expected
- **Documentation**: Tests serve as living documentation of how the code behaves
- **Refactoring Safety**: Good tests allow us to refactor with confidence
- **Bug Prevention**: Tests catch bugs before they reach production
- **Contract Validation**: Tests ensure API contracts are maintained

### Coverage Goals

Our target is **80% code coverage** for both API and web applications, but remember:
- **80% is a starting point**, not an end goal
- **Coverage shows what's NOT tested**, not what IS tested well
- **Prioritize critical paths** over achieving 100% coverage
- **Test behavior, not implementation** - high coverage with poor tests is worse than lower coverage with good tests

## Project Structure

```
sarradabet/
├── apps/
│   ├── api/          # Express API (Jest tests)
│   │   └── src/
│   │       └── __tests__/
│   └── web/          # React app (Vitest tests)
│       └── src/
│           └── __tests__/
└── packages/
    └── types/        # Shared TypeScript types
```

## Running Tests

### From Root Directory

```bash
# Run all tests
npm test

# Run API tests only
npm run test:api

# Run web tests only
npm run test:web

# Run with coverage
npm run test:api:coverage
npm run test:web:coverage
```

### API Tests (Jest)

```bash
cd apps/api

# Run all tests
npm test

# Run in watch mode
npm run test:watch

# Run with coverage
npm run test:coverage

# Run unit tests only
npm run test:unit

# Run integration tests only
npm run test:integration
```

### Web Tests (Vitest)

```bash
cd apps/web

# Run all tests
npm test

# Run in watch mode
npm run test:watch

# Run with coverage
npm run test:coverage

# Run tests once (CI mode)
npm run test:run

# Run with UI
npm run test:ui
```

## Coverage Reports

### Generating Coverage Reports

Coverage is automatically generated when running tests with coverage flags:

```bash
# API coverage
cd apps/api
npm run test:coverage

# Web coverage
cd apps/web
npm run test:coverage
```

### Viewing Coverage Reports

After generating coverage, open the HTML report:

```bash
# API coverage
open apps/api/coverage/index.html  # macOS
xdg-open apps/api/coverage/index.html  # Linux

# Web coverage
open apps/web/coverage/index.html  # macOS
xdg-open apps/web/coverage/index.html  # Linux
```

### Coverage Thresholds

Both applications enforce:
- **Minimum 80% overall coverage** (lines, functions, branches, statements)
- Tests will fail if coverage falls below these thresholds

## API Testing (Jest)

### Test Structure

API tests are located in `apps/api/src/__tests__/` and follow this structure:

```typescript
describe('Feature Name', () => {
  describe('method or function', () => {
    it('should do something', () => {
      // test code
    });
  });
});
```

### Unit Tests

Unit tests test individual functions, services, or modules in isolation:

```typescript
import { BetService } from '../BetService';

describe('BetService', () => {
  describe('createBet', () => {
    it('should create a bet with valid data', async () => {
      const service = new BetService();
      const bet = await service.createBet({
        title: 'Test Bet',
        categoryId: 1,
      });
      
      expect(bet).toBeDefined();
      expect(bet.title).toBe('Test Bet');
    });
  });
});
```

### Integration Tests

Integration tests test API endpoints with a real database:

```typescript
import request from 'supertest';
import { app } from '../../app';

describe('Bet Routes Integration Tests', () => {
  it('should create a bet via POST /api/bets', async () => {
    const response = await request(app)
      .post('/api/bets')
      .send({
        title: 'Test Bet',
        categoryId: 1,
      })
      .expect(201);

    expect(response.body.success).toBe(true);
    expect(response.body.data.title).toBe('Test Bet');
  });
});
```

### Testing with Prisma

When testing with Prisma, use a separate test database:

```typescript
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient({
  datasources: {
    db: {
      url: process.env.DATABASE_URL, // Use test database
    },
  },
});

// Clean up after tests
afterEach(async () => {
  await prisma.bet.deleteMany();
});
```

### Mocking External Services

Use Jest mocks for external services:

```typescript
import { vi } from 'vitest';

// Mock an external service
vi.mock('../externalService', () => ({
  fetchData: vi.fn().mockResolvedValue({ data: 'test' }),
}));
```

## Web Testing (Vitest)

### Test Structure

Web tests are located in `apps/web/src/**/__tests__/` and use Vitest with React Testing Library:

```typescript
import { render, screen } from '@testing-library/react';
import { describe, it, expect } from 'vitest';
import { Component } from '../Component';

describe('Component', () => {
  it('should render correctly', () => {
    render(<Component />);
    expect(screen.getByText('Hello')).toBeInTheDocument();
  });
});
```

### Component Tests

Test React components for:
- **Rendering**: Components render without errors
- **User interactions**: Clicks, form submissions, etc.
- **Props**: Different prop combinations
- **State**: State changes and side effects

Example:

```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from '../Button';

describe('Button', () => {
  it('should render with default props', () => {
    render(<Button>Click me</Button>);
    
    const button = screen.getByRole('button', { name: /click me/i });
    expect(button).toBeInTheDocument();
  });

  it('should call onClick when clicked', () => {
    const handleClick = vi.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

### Hook Tests

Test custom hooks using `renderHook`:

```typescript
import { renderHook, waitFor } from '@testing-library/react';
import { useApi } from '../useApi';

describe('useApi', () => {
  it('should handle successful API call', async () => {
    const apiFunction = vi.fn().mockResolvedValue({
      success: true,
      data: { id: 1 },
    });

    const { result } = renderHook(() => useApi(apiFunction));

    await act(async () => {
      await result.current.execute();
    });

    await waitFor(() => {
      expect(result.current.data).toEqual({ id: 1 });
      expect(result.current.loading).toBe(false);
    });
  });
});
```

### Testing with React Router

When testing components that use React Router:

```typescript
import { BrowserRouter } from 'react-router-dom';

const renderWithRouter = (component: React.ReactElement) => {
  return render(<BrowserRouter>{component}</BrowserRouter>);
};
```

### Mocking API Calls

Mock API calls in component tests:

```typescript
import { vi } from 'vitest';

// Mock fetch
global.fetch = vi.fn().mockResolvedValue({
  ok: true,
  json: async () => ({ success: true, data: [] }),
});
```

## Best Practices

### 1. Test Behavior, Not Implementation

**Bad:**
```typescript
it('should call the service method', () => {
  const service = { calculate: vi.fn() };
  // test implementation
});
```

**Good:**
```typescript
it('should calculate the correct total', () => {
  const result = calculateTotal([10, 20, 30]);
  expect(result).toBe(60);
});
```

### 2. Use Descriptive Test Names

**Bad:**
```typescript
it('works', () => {});
```

**Good:**
```typescript
it('should create a bet with valid data', () => {});
```

### 3. Test Edge Cases

Always test:
- **Empty/null values**
- **Boundary conditions**
- **Invalid inputs**
- **Error conditions**

Example:

```typescript
describe('edge cases', () => {
  it('should handle empty array', () => {
    const result = calculateTotal([]);
    expect(result).toBe(0);
  });

  it('should handle invalid input', () => {
    expect(() => calculateTotal(null)).toThrow();
  });
});
```

### 4. Keep Tests Independent

Each test should be able to run in isolation:
- Use `beforeEach` for setup, not shared state
- Clean up after tests (database, mocks, etc.)
- Don't rely on test execution order

### 5. Use Appropriate Test Types

- **Unit tests**: Fast, test individual functions/components
- **Integration tests**: Test API endpoints with database
- **E2E tests**: Test full user workflows (if applicable)

### 6. Focus on Critical Paths

Prioritize testing:
- **Business-critical functionality**
- **User-facing features**
- **API contracts**
- **Error-prone areas**

## Coverage Best Practices

Based on [Google's Code Coverage Best Practices](https://testing.googleblog.com/2020/08/code-coverage-best-practices.html):

1. **Use coverage to find gaps**: Coverage tells you what's NOT tested, not what IS tested well
2. **Don't chase 100%**: Some code (like error handlers) may not need full coverage
3. **Focus on meaningful coverage**: Test critical paths and edge cases
4. **Avoid testing implementation details**: Test behavior, not how it's implemented
5. **Use coverage as a guide**: It's one metric among many

## CI Integration

### GitHub Actions

Coverage is automatically generated in CI. The workflow:
1. Sets up Node.js and PostgreSQL
2. Installs dependencies
3. Generates Prisma client
4. Prepares the test database
5. Runs tests with coverage for both API and web
6. Uploads coverage artifacts
7. Updates coverage badges

### Coverage Badge

Coverage badges are automatically updated via the `coverage-badge.yml` workflow and displayed in the repository.

## Troubleshooting

### Tests Fail with Coverage Below 80%

1. Check which files have low coverage
2. Review the coverage report
3. Add tests for uncovered code
4. Consider if the code should be excluded (add to coverage excludes)

### Database Issues (API)

```bash
# Reset test database
cd apps/api
npm run db:reset

# Or manually
npx prisma migrate reset --force
npm run db:seed:simple
```

### Prisma Client Not Generated

```bash
cd apps/api
npm run prisma:generate
```

### Module Resolution Issues

Ensure TypeScript paths are correctly configured in `tsconfig.json` and test config files.

### Coverage Not Generating

- Verify coverage flags are set in test commands
- Check that coverage directories are not in `.gitignore` (they should be)
- Ensure coverage providers are installed (`@vitest/coverage-v8` for web, Jest coverage for API)

## Monorepo-Specific Considerations

### Shared Types

When testing, ensure shared types from `packages/types` are properly imported:

```typescript
import { Bet, Category } from '@sarradabet/types';
```

### Turbo Cache

Turbo caches test results. To clear cache:

```bash
# Clear turbo cache
rm -rf .turbo

# Or run tests without cache
npm test -- --no-cache
```

### Running Tests in Parallel

Turbo runs tests in parallel by default. To run sequentially:

```bash
# Run tests one at a time
npm run test:api && npm run test:web
```

## Resources

- [Jest Documentation](https://jestjs.io/)
- [Vitest Documentation](https://vitest.dev/)
- [React Testing Library](https://testing-library.com/react)
- [Supertest Documentation](https://github.com/visionmedia/supertest)
- [Prisma Testing Guide](https://www.prisma.io/docs/guides/testing)
- [Google Testing Blog - Code Coverage Best Practices](https://testing.googleblog.com/2020/08/code-coverage-best-practices.html)

## Questions?

If you have questions about testing or need help writing tests, reach out to the team or check the existing test files for examples.

