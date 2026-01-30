# Component Structure Template

**Purpose**: Standardize component structure, file organization, and coding patterns across all repositories

**Related Documents**:
- [Phase 3 Standardization](./phase-3-standardization.md) - Overall Phase 3 standards
- [Code Review Checklist](./code-review-checklist.md) - Review criteria
- [Grid System Guide](./grid-system-guide.md) - Layout patterns

---

## File Structure

### React Components

All React components should follow this structure:

```
components/
├── ComponentName/
│   ├── ComponentName.tsx       # Main component
│   ├── ComponentName.test.tsx  # Tests (if applicable)
│   ├── ComponentName.stories.tsx # Storybook stories (if applicable)
│   └── index.ts                # Exports
```

**Conventions**:
- Use TypeScript for all new components
- Co-locate tests and stories with components
- Use `index.ts` for clean imports
- Component folder name: PascalCase (e.g., `Button`, `LoadingSpinner`)
- File name: PascalCase matching component name

**Example**:
```typescript
// components/ui/Button/Button.tsx
export function Button({ ... }) { ... }

// components/ui/Button/index.ts
export { Button } from './Button';
export type { ButtonProps } from './Button';
```

### Rails Partials

For Rails projects using ERB views:

```
app/views/
├── shared/
│   ├── _component_name.html.erb  # Partial component
│   └── _component_name.html.erb  # Styles (if needed)
```

**Conventions**:
- Use consistent partial naming: `_component_name.html.erb` (snake_case)
- Co-locate styles with views when using component-based structure
- Use helpers for reusable logic
- Follow Rails conventions for partial organization

---

## React Component Template

### Basic Component Structure

```typescript
import { ComponentProps, forwardRef } from 'react';
import { cn } from '@sarradahub/design-system/utils';
import { designTokens } from '@sarradahub/design-system/tokens';

interface ComponentNameProps extends ComponentProps<'div'> {
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
  'aria-describedby'?: string;
}

export const ComponentName = forwardRef<HTMLDivElement, ComponentNameProps>(
  (
    {
      children,
      variant = 'default',
      size = 'md',
      className,
      onClick,
      'aria-label': ariaLabel,
      ...props
    },
    ref
  ) => {
    return (
      <div
        ref={ref}
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
);

ComponentName.displayName = 'ComponentName';
```

### Key Requirements

- ✅ Use design system tokens for all styling
- ✅ Support `className` prop for composition
- ✅ Include accessibility props (aria-label, role, etc.)
- ✅ Use TypeScript interfaces for props
- ✅ Follow design system prop naming conventions (`variant`, `size`, not `type`, `scale`)
- ✅ Use `forwardRef` when component needs ref forwarding
- ✅ Set `displayName` for better debugging

### Complete Example: ActionCard Component

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
  (
    {
      title,
      description,
      actionLabel,
      onAction,
      variant = 'default',
      className,
      ...props
    },
    ref
  ) => {
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

---

## CSS Methodology Standards

### Standard: Tailwind CSS with Design System Tokens

**Rules**:
- ✅ Use Tailwind utility classes from design system
- ✅ Use CSS variables for design tokens (colors, spacing, typography)
- ✅ No inline styles (except dynamic values)
- ✅ No custom CSS files unless absolutely necessary
- ✅ Use `cn()` utility for conditional class names

### Import cn Utility

```typescript
// From design system (preferred)
import { cn } from '@sarradahub/design-system/utils';

// Or local utility (if design system doesn't export)
import { cn } from '@/utils/cn';
```

### Good Examples

```typescript
// ✅ Good - Tailwind classes with design system tokens
<div className={cn(
  'px-4 py-2',
  'bg-primary-500 text-white',
  'rounded-md',
  'hover:bg-primary-600',
  'focus:outline-none focus:ring-2 focus:ring-primary-500',
  disabled && 'opacity-50 cursor-not-allowed',
  className
)}>

// ✅ Good - Dynamic values in inline styles (acceptable)
<div
  className="w-full h-4 bg-neutral-200 rounded-full"
  style={{ width: `${progress}%` }}
>

// ✅ Good - Using design system spacing tokens
<div className="space-y-4"> {/* Uses design system spacing */}
  <div className="p-6">Content</div>
</div>
```

### Bad Examples

```typescript
// ❌ Bad - Inline styles for static values
<div style={{ padding: '16px', backgroundColor: '#3b82f6' }}>

// ❌ Bad - Hardcoded colors
<div className="bg-[#3b82f6] text-white">

// ❌ Bad - Custom CSS file for simple styling
// styles.css
.custom-button { padding: 16px; }

// ❌ Bad - Not using cn utility
<div className={`base ${variant === 'primary' ? 'primary' : ''} ${className}`}>
```

---

## Prop API Conventions

### Design System Prop Naming

Follow design system prop naming conventions:

| Design System | Custom Names (Avoid) |
|--------------|---------------------|
| `variant` | `type`, `style`, `appearance` |
| `size` | `scale`, `dimension` |
| `disabled` | `isDisabled`, `enabled` |
| `onClick` | `handleClick`, `clickHandler` |
| `className` | `class`, `cssClass` |
| `children` | `content`, `body` |

### Required Props Pattern

**Always include**:
- `children`: For container components
- `className`: For style composition
- Accessibility props: `aria-label`, `role`, etc. when needed

### Optional Props Pattern

- Provide sensible defaults
- Use TypeScript optional syntax (`prop?: type`)
- Document in JSDoc comments

### Example: Proper Prop API

```typescript
interface ButtonProps extends ComponentProps<'button'> {
  // Required
  children: React.ReactNode;
  
  // Optional with defaults
  variant?: 'primary' | 'secondary' | 'text' | 'danger' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  loading?: boolean;
  
  // Event handlers
  onClick?: () => void;
  
  // Composition
  className?: string;
  
  // Accessibility
  'aria-label'?: string;
  'aria-describedby'?: string;
}
```

---

## Composition Patterns

### Prefer Composition Over Configuration

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

### Component Composition Rules

- ✅ Use compound components for complex UI
- ✅ Allow `children` prop for flexibility
- ✅ Use render props or hooks for logic
- ✅ Keep components focused and small
- ✅ Compose complex components from simple ones

### Example: Compound Components

```typescript
// Card compound component
export const Card = ({ children, className, ...props }) => (
  <div className={cn('rounded-lg border bg-white', className)} {...props}>
    {children}
  </div>
);

export const CardHeader = ({ children, className, ...props }) => (
  <div className={cn('p-6 border-b', className)} {...props}>
    {children}
  </div>
);

export const CardContent = ({ children, className, ...props }) => (
  <div className={cn('p-6', className)} {...props}>
    {children}
  </div>
);

// Usage
<Card>
  <CardHeader>
    <h2>Title</h2>
  </CardHeader>
  <CardContent>
    <p>Content</p>
  </CardContent>
</Card>
```

---

## Rails Partial Template

### ERB Partial Structure

```erb
<%# app/views/shared/_action_card.html.erb %>
<%# 
  Partial: Action Card
  Purpose: Displays a card with title, description, and action button
  
  Parameters:
    - title (required): Card title
    - description (optional): Card description
    - action_label (required): Button label
    - action_path (required): Button link path
    - variant (optional): 'default' or 'primary'
-%>

<div class="<%= cn(
  'col-span-1',
  'p-6 rounded-lg border border-neutral-200',
  'bg-white',
  variant == 'primary' ? 'border-primary-500 bg-primary-50' : '',
  local_assigns[:class]
) %>">
  <h3 class="text-lg font-semibold mb-2"><%= title %></h3>
  <% if description.present? %>
    <p class="text-sm text-neutral-600 mb-4"><%= description %></p>
  <% end %>
  
  <%= link_to action_label, action_path,
    class: "inline-flex items-center justify-center rounded-lg px-4 py-2 font-medium transition-colors #{variant == 'primary' ? 'bg-primary-600 text-white hover:bg-primary-700' : 'bg-neutral-200 text-neutral-900 hover:bg-neutral-300'}",
    aria: { label: "#{action_label} for #{title}" }
  %>
</div>
```

### Rails Helper for cn Utility

```ruby
# app/helpers/application_helper.rb
def cn(*classes)
  classes.compact.reject(&:blank?).join(' ')
end
```

### Rails Conventions

- Use snake_case for partial names: `_component_name.html.erb`
- Document parameters in comments
- Use `local_assigns` for optional parameters
- Use Rails helpers for common patterns
- Follow Rails view organization conventions

---

## Accessibility Requirements

### Required Accessibility Props

All interactive components must include:

- **Keyboard navigation**: All interactive elements accessible via keyboard
- **ARIA labels**: Interactive elements without visible text need `aria-label`
- **Semantic HTML**: Use correct HTML elements (`button`, not `div` with `onClick`)
- **Focus management**: Visible focus indicators
- **Form accessibility**: Labels associated with inputs (`htmlFor`/`id`)

### Example: Accessible Button

```typescript
<button
  onClick={handleClick}
  aria-label="Close dialog"
  className="focus:outline-none focus:ring-2 focus:ring-primary-500"
>
  <XIcon aria-hidden="true" />
</button>
```

### Example: Accessible Form Input

```typescript
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
```

---

## Testing Requirements

### Component Tests

```typescript
// ComponentName.test.tsx
import { render, screen } from '@testing-library/react';
import { ComponentName } from './ComponentName';

describe('ComponentName', () => {
  it('renders correctly', () => {
    render(<ComponentName>Content</ComponentName>);
    expect(screen.getByText('Content')).toBeInTheDocument();
  });

  it('applies variant classes', () => {
    const { container } = render(
      <ComponentName variant="primary">Content</ComponentName>
    );
    expect(container.firstChild).toHaveClass('primary-styles');
  });

  it('is accessible', () => {
    render(<ComponentName aria-label="Test">Content</ComponentName>);
    expect(screen.getByLabelText('Test')).toBeInTheDocument();
  });
});
```

---

## Migration Checklist

When updating existing components to Phase 3 standards:

- [ ] **Update file structure**: Follow standard structure
- [ ] **Replace hardcoded values**: Use design system tokens
- [ ] **Update prop API**: Follow design system conventions
- [ ] **Add accessibility**: Include required a11y props
- [ ] **Update styling**: Use Tailwind classes from design system
- [ ] **Add tests**: Include accessibility and functionality tests
- [ ] **Update documentation**: Document new API and usage

---

## Resources

- [Design System Package](../../platform/design-system) - Component library
- [Phase 3 Standardization](./phase-3-standardization.md) - Overall standards
- [Code Review Checklist](./code-review-checklist.md) - Review criteria
- [UX Principles Checklist](./ux-principles-checklist.md) - Quality standards

---

**Last Updated**: 2025-01-XX  
**Maintained By**: Design System Team

