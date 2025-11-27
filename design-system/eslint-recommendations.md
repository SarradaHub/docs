# ESLint Recommendations for Design System Standards

**Purpose**: Recommended ESLint rules and configurations to enforce Phase 3 design system standards

**Status**: Recommendations (optional implementation)

**Related Documents**:
- [Code Review Checklist](./code-review-checklist.md) - Manual review standards
- [Phase 3 Standardization](./phase-3-standardization.md) - Overall standards
- [Component Structure Template](./component-structure-template.md) - Component standards

---

## Overview

While manual code review is the primary enforcement mechanism for Phase 3 standards, ESLint rules can help catch common issues automatically. This document provides recommendations for ESLint configurations that support design system standards.

**Note**: Some rules may require custom ESLint plugins or may not be directly enforceable via ESLint. These are recommendations that can be implemented incrementally.

---

## Recommended Rules

### 1. Accessibility Rules

#### eslint-plugin-jsx-a11y

Install:
```bash
npm install --save-dev eslint-plugin-jsx-a11y
```

Recommended rules:
```javascript
{
  "rules": {
    // Require ARIA labels on interactive elements
    "jsx-a11y/aria-label": "warn",
    "jsx-a11y/aria-props": "error",
    "jsx-a11y/aria-proptypes": "error",
    "jsx-a11y/aria-unsupported-elements": "error",
    
    // Require semantic HTML
    "jsx-a11y/click-events-have-key-events": "warn",
    "jsx-a11y/no-static-element-interactions": "warn",
    "jsx-a11y/interactive-supports-focus": "warn",
    
    // Form accessibility
    "jsx-a11y/label-has-associated-control": "error",
    "jsx-a11y/label-has-for": "off", // Deprecated, use label-has-associated-control
    
    // Image accessibility
    "jsx-a11y/alt-text": "error",
    "jsx-a11y/img-redundant-alt": "warn",
    
    // Focus management
    "jsx-a11y/no-autofocus": "warn",
    "jsx-a11y/no-noninteractive-tabindex": "warn"
  }
}
```

### 2. Design System Import Rules

#### Custom Rule (if needed)

Prefer design system imports over local implementations:

```javascript
// ✅ Good
import { Button } from "@sarradahub/design-system";
import { cn } from "@sarradahub/design-system/utils";

// ❌ Bad (if design system has equivalent)
import { Button } from "./components/ui/Button";
```

**Implementation**: This may require a custom ESLint rule or can be enforced via code review.

### 3. Inline Styles Rule

Disallow inline styles (except dynamic values):

```javascript
{
  "rules": {
    // Using react/no-unknown-property or similar
    "react/no-unknown-property": ["error", {
      "ignore": ["style"] // Allow style for dynamic values only
    }]
  }
}
```

**Note**: This is difficult to enforce perfectly via ESLint. Consider using a comment-based approach:
```javascript
// eslint-disable-next-line react/no-unknown-property
<div style={{ width: `${progress}%` }}> // Dynamic value - OK
```

### 4. Prop Naming Conventions

Enforce design system prop naming:

```javascript
// Custom rule needed or manual review
// Prefer: variant, size, disabled
// Avoid: type, scale, isDisabled
```

**Implementation**: This requires a custom ESLint rule or can be enforced via code review.

### 5. Hardcoded Values

Warn about hardcoded colors/spacing:

```javascript
// Custom rule needed
// Warn on: className="bg-[#3b82f6]"
// Allow: className="bg-primary-600"
```

**Implementation**: This requires a custom ESLint rule or regex-based approach.

---

## Example ESLint Configuration

### For React Projects (saturday_league_football_frontend, sarradabet)

```javascript
// eslint.config.js
import js from '@eslint/js';
import react from 'eslint-plugin-react';
import reactHooks from 'eslint-plugin-react-hooks';
import jsxA11y from 'eslint-plugin-jsx-a11y';
import typescript from '@typescript-eslint/eslint-plugin';
import typescriptParser from '@typescript-eslint/parser';

export default [
  js.configs.recommended,
  {
    files: ['**/*.{js,jsx,ts,tsx}'],
    languageOptions: {
      parser: typescriptParser,
      parserOptions: {
        ecmaVersion: 'latest',
        sourceType: 'module',
        ecmaFeatures: {
          jsx: true,
        },
      },
    },
    plugins: {
      react,
      'react-hooks': reactHooks,
      'jsx-a11y': jsxA11y,
      '@typescript-eslint': typescript,
    },
    rules: {
      // React rules
      ...react.configs.recommended.rules,
      'react/react-in-jsx-scope': 'off', // Not needed in React 17+
      'react/prop-types': 'off', // Using TypeScript instead
      
      // React Hooks
      ...reactHooks.configs.recommended.rules,
      
      // Accessibility
      ...jsxA11y.configs.recommended.rules,
      'jsx-a11y/aria-label': 'warn',
      'jsx-a11y/label-has-associated-control': 'error',
      'jsx-a11y/click-events-have-key-events': 'warn',
      'jsx-a11y/no-static-element-interactions': 'warn',
      
      // TypeScript
      '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
      
      // Design System Standards (manual review recommended)
      // These are difficult to enforce via ESLint:
      // - Design system import preferences
      // - Prop naming conventions (variant vs type)
      // - Hardcoded color/spacing values
    },
    settings: {
      react: {
        version: 'detect',
      },
    },
  },
];
```

---

## Implementation Strategy

### Phase 1: Basic Rules (Recommended)
1. Install `eslint-plugin-jsx-a11y`
2. Enable accessibility rules
3. Enable React/TypeScript rules

### Phase 2: Custom Rules (Optional)
1. Create custom ESLint rules for design system standards
2. Or use regex-based rules where possible
3. Document exceptions and edge cases

### Phase 3: Pre-commit Hooks (Optional)
1. Run ESLint on pre-commit
2. Block commits with accessibility errors
3. Warn on design system violations

---

## Alternative: Code Review Focus

Since many design system standards are difficult to enforce via ESLint, the primary enforcement mechanism is:

1. **Code Review Checklist**: Use [Code Review Checklist](./code-review-checklist.md) for all PRs
2. **Manual Review**: Reviewers check for:
   - Design system component usage
   - Prop naming conventions
   - Hardcoded values
   - Accessibility compliance
3. **Documentation**: Reference [Component Structure Template](./component-structure-template.md) during reviews

---

## Tools & Resources

- [eslint-plugin-jsx-a11y](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y) - Accessibility linting
- [eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react) - React-specific linting
- [@typescript-eslint/eslint-plugin](https://typescript-eslint.io/) - TypeScript linting
- [Writing Custom ESLint Rules](https://eslint.org/docs/latest/extend/custom-rules) - For custom design system rules

---

## Notes

- ESLint rules are **recommendations**, not requirements
- Code review remains the primary enforcement mechanism
- Focus on rules that can be automatically enforced (accessibility, syntax)
- Manual review for design system-specific standards (component usage, prop naming)
- Custom rules can be added incrementally as needed

---

**Last Updated**: 2025-01-XX  
**Maintained By**: Design System Team

