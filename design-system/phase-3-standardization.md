# Phase 3: Standardization of Structure & Best Practices

**Status**: In Progress  
**Purpose**: Enforce consistent structure, patterns, and best practices for all new and updated components/pages

**Related Documents**:
- [UX Principles Checklist](./ux-principles-checklist.md) - Quality standards to enforce
- [Phase 2 Audit Checklist](./phase-2-audit-checklist.md) - Integration verification
- [Phase 4 Rollout Strategy](./phase-4-rollout-strategy.md) - Implementation approach
- [Implementation Summary](./implementation-summary.md) - Overall progress

---

## Overview

Phase 3 goes beyond visual consistency to enforce structural standards, layout patterns, and UX best practices. All new and updated components/pages must follow these standards.

### Key Focus Areas

1. **Component Structure**: Standardize internal structure, CSS methodology, prop APIs, and composition patterns
2. **Layout Grid**: Implement consistent 12-column grid system across all pages
3. **UX Principles Integration**: Enforce best practices from UX Principles Checklist
4. **Code Quality**: Ensure consistent code patterns and maintainability

---

## 1. Component Structure Standards

### 1.1 File Structure

All components should follow a consistent file structure:

```
components/
├── ComponentName/
│   ├── ComponentName.tsx       # Main component
│   ├── ComponentName.test.tsx # Tests (if applicable)
│   ├── ComponentName.stories.tsx # Storybook stories (if applicable)
│   └── index.ts                # Exports
```

**For React Projects**:
- Use TypeScript for all new components
- Co-locate tests and stories with components
- Use index.ts for clean imports

**For Rails Projects**:
- Use consistent partial naming: `_component_name.html.erb`
- Co-locate styles with views when using component-based structure
- Use helpers for reusable logic

### 1.2 Component Structure Template

#### React Component Template

```typescript
import { ComponentProps } from 'react';
import { cn } from '@sarradahub/design-system/utils'; // or local utility
import { designTokens } from '@sarradahub/design-system/tokens';

interface ComponentNameProps {
  // Required props
  children: React.ReactNode;
  
  // Optional props with defaults
  variant?: 'default' | 'primary' | 'secondary';
  size?: 'sm' | 'md' | 'lg';
  className?: string;
  
  // Event handlers
  onClick?: () => void;
  
  // Accessibility
  'aria-label'?: string;
}

export function ComponentName({
  children,
  variant = 'default',
  size = 'md',
  className,
  onClick,
  'aria-label': ariaLabel,
  ...props
}: ComponentNameProps) {
  return (
    <div
      className={cn(
        // Base styles using design system tokens
        'base-styles',
        // Variant styles
        variant === 'primary' && 'primary-styles',
        // Size styles
        size === 'sm' && 'text-sm',
        // Custom className
        className
      )}
      onClick={onClick}
      aria-label={ariaLabel}
      {...props}
    >
      {children}
    </div>
  );
}
```

**Key Requirements**:
- ✅ Use design system tokens for all styling
- ✅ Support className prop for composition
- ✅ Include accessibility props (aria-label, role, etc.)
- ✅ Use TypeScript interfaces for props
- ✅ Follow design system prop naming conventions (`variant`, `size`, not `type`, `scale`)

### 1.3 CSS Methodology

**Standard**: Tailwind CSS with design system tokens

**Rules**:
- ✅ Use Tailwind utility classes from design system
- ✅ Use CSS variables for design tokens (colors, spacing, typography)
- ✅ No inline styles (except dynamic values)
- ✅ No custom CSS files unless absolutely necessary
- ✅ Use `cn()` utility for conditional class names

**Example**:
```typescript
// ✅ Good
<div className={cn(
  'px-4 py-2',
  'bg-primary-500 text-white',
  'rounded-md',
  'hover:bg-primary-600',
  disabled && 'opacity-50 cursor-not-allowed',
  className
)}>

// ❌ Bad
<div style={{ padding: '16px', backgroundColor: '#3b82f6' }}>
```

### 1.4 Prop API Conventions

Follow design system prop naming conventions:

| Design System | Custom Names (Avoid) |
|--------------|---------------------|
| `variant` | `type`, `style`, `appearance` |
| `size` | `scale`, `dimension` |
| `disabled` | `isDisabled`, `enabled` |
| `onClick` | `handleClick`, `clickHandler` |
| `className` | `class`, `cssClass` |

**Required Props Pattern**:
- `children`: For container components
- `className`: For style composition
- Accessibility props: `aria-label`, `role`, etc. when needed

**Optional Props Pattern**:
- Provide sensible defaults
- Use TypeScript optional syntax (`prop?: type`)
- Document in JSDoc comments

### 1.5 Composition Patterns

**Prefer composition over configuration**:

```typescript
// ✅ Good - Composition
<Card>
  <CardHeader>
    <CardTitle>Title</CardTitle>
  </CardHeader>
  <CardContent>
    Content
  </CardContent>
</Card>

// ❌ Bad - Configuration
<Card title="Title" content="Content" />
```

**Component Composition Rules**:
- ✅ Use compound components for complex UI
- ✅ Allow children prop for flexibility
- ✅ Use render props or hooks for logic
- ✅ Keep components focused and small

---

## 2. Layout Grid Implementation

### 2.1 12-Column Grid System

All pages must use a consistent 12-column grid system for visual alignment and rhythm.

#### Grid Specifications

- **Columns**: 12 columns
- **Gutters**: Consistent spacing between columns (using design system spacing tokens)
- **Max-width container**: Content doesn't stretch too wide on large screens
- **Breakpoints**: Use design system breakpoints

#### Implementation

**For React Projects** (using Tailwind):

```tsx
// Container with max-width
<div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
  {/* 12-column grid */}
  <div className="grid grid-cols-1 md:grid-cols-12 gap-4 md:gap-6">
    {/* Sidebar - 3 columns */}
    <aside className="md:col-span-3">
      Sidebar content
    </aside>
    
    {/* Main content - 9 columns */}
    <main className="md:col-span-9">
      Main content
    </main>
  </div>
</div>
```

**For Rails Projects** (using Tailwind):

```erb
<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
  <div class="grid grid-cols-1 md:grid-cols-12 gap-4 md:gap-6">
    <aside class="md:col-span-3">
      <%= render 'sidebar' %>
    </aside>
    
    <main class="md:col-span-9">
      <%= yield %>
    </main>
  </div>
</div>
```

### 2.2 Grid Usage Rules

- ✅ **Always use grid for multi-column layouts**: Don't use flexbox for column-based layouts
- ✅ **Align to grid**: All elements align to grid columns
- ✅ **Consistent gutters**: Use design system spacing tokens for gaps
- ✅ **Responsive breakpoints**: Grid adapts at design system breakpoints
- ✅ **Max-width containers**: Prevent content from stretching too wide

### 2.3 Common Layout Patterns

#### Two-Column Layout (Sidebar + Content)
```tsx
<div className="grid grid-cols-1 md:grid-cols-12 gap-6">
  <aside className="md:col-span-3">Sidebar</aside>
  <main className="md:col-span-9">Content</main>
</div>
```

#### Three-Column Layout
```tsx
<div className="grid grid-cols-1 md:grid-cols-12 gap-6">
  <aside className="md:col-span-2">Left sidebar</aside>
  <main className="md:col-span-8">Content</main>
  <aside className="md:col-span-2">Right sidebar</aside>
</div>
```

#### Card Grid
```tsx
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
  {items.map(item => <Card key={item.id}>{item.content}</Card>)}
</div>
```

### 2.4 Visual Alignment

- ✅ **Elements align to grid**: All major elements align to grid columns
- ✅ **Consistent spacing**: Use design system spacing scale
- ✅ **Visual rhythm**: Consistent spacing creates visual rhythm
- ✅ **No arbitrary spacing**: Avoid random spacing values

---

## 3. UX Principles Integration

All components and pages must follow the [UX Principles Checklist](./ux-principles-checklist.md). This section highlights the four key areas for Phase 3.

### 3.1 Clarity & Hierarchy

**Enforcement**:
- ✅ **Purpose immediately clear**: Component/page purpose is obvious
- ✅ **Single primary action**: One main CTA per view
- ✅ **Visual hierarchy**: Most important information is most prominent
- ✅ **Adequate white space**: Key elements have breathing room
- ✅ **Logical grouping**: Related information grouped together

**Code Review Checklist**:
- [ ] Component purpose is clear from props/API
- [ ] Page has one primary action
- [ ] Visual hierarchy uses size, color, position effectively
- [ ] Spacing uses design system tokens
- [ ] Related elements are visually grouped

**Examples**:
```tsx
// ✅ Good - Clear hierarchy
<div className="space-y-6">
  <h1 className="text-3xl font-bold">Page Title</h1>
  <p className="text-lg text-neutral-600">Description</p>
  <div className="flex gap-4">
    <Button variant="primary" size="lg">Primary Action</Button>
    <Button variant="outline">Secondary</Button>
  </div>
</div>

// ❌ Bad - No hierarchy
<div>
  <h1>Title</h1>
  <p>Description</p>
  <button>Action 1</button>
  <button>Action 2</button>
  <button>Action 3</button>
</div>
```

### 3.2 User Journey & Flow

**Enforcement**:
- ✅ **Intuitive user path**: Users can complete tasks without confusion
- ✅ **Reduced cognitive load**: Information presented in digestible chunks
- ✅ **Clear next steps**: Users know what to do next
- ✅ **Minimal steps**: User journey optimized (fewest clicks/actions)
- ✅ **Escape hatches**: Users can easily go back or cancel

**Code Review Checklist**:
- [ ] User flow is logical and intuitive
- [ ] Forms are optimized (only necessary fields)
- [ ] Multi-step processes broken into logical steps
- [ ] Navigation is clear (breadcrumbs, active states)
- [ ] Cancel/back options available

**Form Optimization**:
```tsx
// ✅ Good - Optimized form
<form className="space-y-4">
  <Input label="Email" type="email" required />
  <Input label="Password" type="password" required />
  <Button type="submit">Sign In</Button>
</form>

// ❌ Bad - Too many fields
<form>
  <Input label="First Name" />
  <Input label="Middle Name" />
  <Input label="Last Name" />
  <Input label="Email" />
  <Input label="Confirm Email" />
  <Input label="Phone" />
  <Input label="Address" />
  <Input label="City" />
  <Input label="State" />
  <Input label="Zip" />
  <Input label="Country" />
  <Button>Submit</Button>
</form>
```

### 3.3 Accessibility (a11y)

**Enforcement**:
- ✅ **Keyboard navigable**: All interactive elements accessible via keyboard
- ✅ **Screen reader friendly**: Semantic HTML, ARIA labels, roles
- ✅ **Color contrast**: Meets WCAG AA (4.5:1 for normal, 3:1 for large)
- ✅ **Not color-dependent**: Information not conveyed by color alone
- ✅ **Form accessibility**: Labels associated, errors announced

**Code Review Checklist**:
- [ ] All interactive elements keyboard accessible
- [ ] Focus indicators visible
- [ ] Semantic HTML used (button, not div with onClick)
- [ ] ARIA labels on interactive elements
- [ ] Form labels associated (for/id)
- [ ] Color contrast meets WCAG AA
- [ ] Errors associated with form fields

**Examples**:
```tsx
// ✅ Good - Accessible
<button
  onClick={handleClick}
  aria-label="Close dialog"
  className="focus:outline-none focus:ring-2 focus:ring-primary-500"
>
  <XIcon aria-hidden="true" />
</button>

<label htmlFor="email">Email</label>
<input
  id="email"
  type="email"
  aria-describedby="email-error"
  aria-invalid={hasError}
/>
{hasError && (
  <span id="email-error" role="alert" className="text-error-500">
    Invalid email address
  </span>
)}

// ❌ Bad - Not accessible
<div onClick={handleClick}>
  <XIcon />
</div>

<input type="email" />
{hasError && <span>Error</span>}
```

### 3.4 Consistency & Feedback

**Enforcement**:
- ✅ **Design system components**: No custom components when design system has equivalent
- ✅ **Consistent patterns**: Same patterns used for similar actions
- ✅ **Immediate feedback**: All interactions provide instant feedback
- ✅ **Loading states**: Actions >500ms show loading indicators
- ✅ **Error handling**: Clear, actionable error messages

**Code Review Checklist**:
- [ ] Uses design system components
- [ ] Button styles consistent for similar actions
- [ ] Hover/active states consistent
- [ ] Loading states implemented
- [ ] Error messages are specific and actionable
- [ ] Success confirmations for significant actions

**Examples**:
```tsx
// ✅ Good - Consistent feedback
<Button
  onClick={handleSubmit}
  disabled={isLoading}
  variant="primary"
>
  {isLoading ? (
    <>
      <Loader2 className="animate-spin mr-2" />
      Saving...
    </>
  ) : (
    'Save Changes'
  )}
</Button>

{error && (
  <Alert variant="error">
    <AlertTitle>Error</AlertTitle>
    <AlertDescription>
      Failed to save changes. Please check your connection and try again.
    </AlertDescription>
  </Alert>
)}

// ❌ Bad - No feedback
<button onClick={handleSubmit}>Save</button>
{error && <div>Error</div>}
```

---

## 4. Enforcement Mechanisms

### 4.1 Code Review Checklist

Every PR must be reviewed against this checklist:

#### Component Structure
- [ ] Follows component structure template
- [ ] Uses design system tokens
- [ ] Props follow design system conventions
- [ ] TypeScript types defined (for TS projects)
- [ ] Accessibility props included

#### Layout & Grid
- [ ] Uses 12-column grid system
- [ ] Elements align to grid
- [ ] Consistent spacing (design system tokens)
- [ ] Max-width container used

#### UX Principles
- [ ] Clarity & hierarchy checked
- [ ] User journey optimized
- [ ] Accessibility requirements met
- [ ] Feedback mechanisms implemented

#### Code Quality
- [ ] No duplicate code
- [ ] No hardcoded values (colors, spacing)
- [ ] Tests included (if applicable)
- [ ] Documentation updated

### 4.2 Linting Rules

**Recommended ESLint/TypeScript Rules**:
- Enforce design system imports
- Disallow inline styles
- Require accessibility props
- Enforce prop naming conventions

**Example ESLint Config**:
```json
{
  "rules": {
    "no-inline-styles": "error",
    "require-aria-label": "warn",
    "prefer-design-system-tokens": "warn"
  }
}
```

### 4.3 Pre-commit Hooks

**Recommended Checks**:
- TypeScript type checking
- Linting
- Accessibility checks (if tools available)
- Design system import validation

### 4.4 Documentation Requirements

**Required Documentation**:
- Component API documentation (props, examples)
- Usage examples
- Accessibility notes
- Migration notes (if replacing existing component)

---

## 5. Migration Guide for Existing Components

### 5.1 Migration Checklist

When updating existing components to Phase 3 standards:

- [ ] **Update file structure**: Follow standard structure
- [ ] **Replace hardcoded values**: Use design system tokens
- [ ] **Update prop API**: Follow design system conventions
- [ ] **Add accessibility**: Include required a11y props
- [ ] **Update styling**: Use Tailwind classes from design system
- [ ] **Add tests**: Include accessibility and functionality tests
- [ ] **Update documentation**: Document new API and usage

### 5.2 Migration Priority

1. **High Priority**: Components used everywhere (Button, Input, Modal)
2. **Medium Priority**: Components used frequently (Card, Alert, Select)
3. **Low Priority**: Components used occasionally (specialized components)

### 5.3 Breaking Changes

**When to introduce breaking changes**:
- Major version updates
- Significant UX improvements
- Security/accessibility fixes

**Breaking change process**:
1. Document breaking changes
2. Provide migration guide
3. Support both versions during transition (if possible)
4. Update all usages

---

## 6. Examples and Best Practices

### 6.1 Complete Component Example

```typescript
import { ComponentProps, forwardRef } from 'react';
import { cn } from '@sarradahub/design-system/utils';
import { Button } from '@sarradahub/design-system/components';

interface ActionCardProps extends ComponentProps<'div'> {
  title: string;
  description?: string;
  actionLabel: string;
  onAction: () => void;
  variant?: 'default' | 'primary';
}

export const ActionCard = forwardRef<HTMLDivElement, ActionCardProps>(
  ({ title, description, actionLabel, onAction, variant = 'default', className, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(
          // Grid alignment
          'col-span-1',
          // Base styles
          'p-6 rounded-lg border border-neutral-200',
          'bg-white',
          // Variant styles
          variant === 'primary' && 'border-primary-500 bg-primary-50',
          className
        )}
        {...props}
      >
        {/* Clear hierarchy */}
        <h3 className="text-lg font-semibold mb-2">{title}</h3>
        {description && (
          <p className="text-sm text-neutral-600 mb-4">{description}</p>
        )}
        
        {/* Single primary action */}
        <Button
          variant={variant === 'primary' ? 'primary' : 'outline'}
          onClick={onAction}
          aria-label={`${actionLabel} for ${title}`}
        >
          {actionLabel}
        </Button>
      </div>
    );
  }
);

ActionCard.displayName = 'ActionCard';
```

### 6.2 Page Layout Example

```typescript
export function DashboardPage() {
  return (
    {/* Max-width container */}
    <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      {/* 12-column grid */}
      <div className="grid grid-cols-1 md:grid-cols-12 gap-6">
        {/* Sidebar - 3 columns */}
        <aside className="md:col-span-3">
          <Navigation />
        </aside>
        
        {/* Main content - 9 columns */}
        <main className="md:col-span-9">
          {/* Clear hierarchy */}
          <h1 className="text-3xl font-bold mb-6">Dashboard</h1>
          
          {/* Card grid - responsive */}
          <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 mb-8">
            <StatCard title="Users" value="1,234" />
            <StatCard title="Revenue" value="$12,345" />
            <StatCard title="Growth" value="+12%" />
          </div>
          
          {/* Primary content area */}
          <Card>
            <CardHeader>
              <CardTitle>Recent Activity</CardTitle>
            </CardHeader>
            <CardContent>
              {/* Content */}
            </CardContent>
          </Card>
        </main>
      </div>
    </div>
  );
}
```

---

## 7. Quality Gates

### 7.1 Definition of Done

A component/page is considered "done" when:

- ✅ Follows component structure standards
- ✅ Uses 12-column grid system
- ✅ Meets all UX principles checklist items
- ✅ Passes accessibility checks (keyboard, screen reader, contrast)
- ✅ Has tests (if applicable)
- ✅ Documentation updated
- ✅ Code review approved
- ✅ No hardcoded values (colors, spacing)

### 7.2 Quality Metrics

**Track these metrics**:
- Component structure compliance: % of components following standards
- Grid usage: % of pages using 12-column grid
- Accessibility score: % of components meeting a11y requirements
- Design system usage: % of components using design system tokens

---

## 8. Resources

### 8.1 Design System Resources

- Design System Package: `platform/design-system`
- Storybook: Local Storybook instance (if available)
- Design Tokens: `@sarradahub/design-system/tokens`
- Components: `@sarradahub/design-system/components`

### 8.2 Reference Documents

- [UX Principles Checklist](./ux-principles-checklist.md) - Detailed quality standards
- [Phase 2 Audit Checklist](./phase-2-audit-checklist.md) - Integration verification
- [Phase 4 Rollout Strategy](./phase-4-rollout-strategy.md) - Implementation approach

### 8.3 Tools

- Tailwind CSS: Utility-first CSS framework
- TypeScript: Type safety
- ESLint: Code linting
- Accessibility tools: axe DevTools, WAVE, Lighthouse

---

## Notes

_Use this space for any additional standards, patterns, or considerations:_

_______________________________________________________________________
_______________________________________________________________________
_______________________________________________________________________

---

**Last Updated**: _______________________  
**Maintained By**: _______________________

