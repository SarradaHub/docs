# Component Documentation Template

**Purpose**: Template for documenting components according to Phase 3 standards

**Related Documents**:
- [Component Structure Template](./component-structure-template.md) - Component standards
- [Code Review Checklist](./code-review-checklist.md) - Review criteria

---

## Component Documentation Template

Use this template when documenting new or updated components.

```markdown
# ComponentName

**Purpose**: Brief description of what the component does

**Status**: ✅ Stable | ⏳ In Progress | 📋 Planned

**Related Components**: [RelatedComponent1](./RelatedComponent1.md), [RelatedComponent2](./RelatedComponent2.md)

---

## Overview

Brief description of the component's purpose and when to use it.

---

## API

### Props

| Prop | Type | Default | Required | Description |
|------|------|---------|----------|-------------|
| `children` | `React.ReactNode` | - | Yes | Component content |
| `variant` | `'default' \| 'primary' \| 'secondary'` | `'default'` | No | Visual variant |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | No | Component size |
| `className` | `string` | - | No | Additional CSS classes |
| `onClick` | `() => void` | - | No | Click handler |
| `aria-label` | `string` | - | No | Accessibility label |

### Example

\`\`\`tsx
import { ComponentName } from '@sarradahub/design-system/components';

<ComponentName variant="primary" size="lg" onClick={handleClick}>
  Click me
</ComponentName>
\`\`\`

---

## Variants

### Default

\`\`\`tsx
<ComponentName>Default variant</ComponentName>
\`\`\`

### Primary

\`\`\`tsx
<ComponentName variant="primary">Primary variant</ComponentName>
\`\`\`

### Secondary

\`\`\`tsx
<ComponentName variant="secondary">Secondary variant</ComponentName>
\`\`\`

---

## Sizes

### Small

\`\`\`tsx
<ComponentName size="sm">Small</ComponentName>
\`\`\`

### Medium

\`\`\`tsx
<ComponentName size="md">Medium</ComponentName>
\`\`\`

### Large

\`\`\`tsx
<ComponentName size="lg">Large</ComponentName>
\`\`\`

---

## Usage Examples

### Basic Usage

\`\`\`tsx
<ComponentName>Basic usage</ComponentName>
\`\`\`

### With Event Handler

\`\`\`tsx
<ComponentName onClick={() => console.log('clicked')}>
  Click me
</ComponentName>
\`\`\`

### With Custom Styling

\`\`\`tsx
<ComponentName className="custom-class">
  Custom styled
</ComponentName>
\`\`\`

---

## Accessibility

### Keyboard Navigation

- Component is fully keyboard navigable
- Tab order is logical
- Focus indicators are visible

### Screen Reader Support

- Semantic HTML used
- ARIA labels provided where needed
- ARIA roles applied correctly

### Color Contrast

- Meets WCAG AA standards (4.5:1 for normal text, 3:1 for large text)
- Information not conveyed by color alone

---

## Migration Notes

_If replacing an existing component, document migration steps here:_

### From OldComponent

1. Replace import: `import { OldComponent } from './OldComponent'` → `import { ComponentName } from '@sarradahub/design-system/components'`
2. Update props: Map old props to new props
3. Update styling: Remove custom styles, use design system tokens
4. Test: Verify functionality and appearance

### Breaking Changes

- List any breaking changes
- Provide migration examples

---

## Implementation Details

### File Structure

\`\`\`
components/
├── ComponentName/
│   ├── ComponentName.tsx
│   ├── ComponentName.test.tsx
│   ├── ComponentName.stories.tsx
│   └── index.ts
\`\`\`

### Dependencies

- `@sarradahub/design-system/utils` - For `cn` utility
- `@sarradahub/design-system/tokens` - For design tokens

---

## Testing

### Test Coverage

- Unit tests: ✅ Complete
- Accessibility tests: ✅ Complete
- Visual regression tests: ⏳ Pending

### Test Examples

\`\`\`tsx
import { render, screen } from '@testing-library/react';
import { ComponentName } from './ComponentName';

describe('ComponentName', () => {
  it('renders correctly', () => {
    render(<ComponentName>Content</ComponentName>);
    expect(screen.getByText('Content')).toBeInTheDocument();
  });
});
\`\`\`

---

## Related Documentation

- [Component Structure Template](./component-structure-template.md)
- [Code Review Checklist](./code-review-checklist.md)
- [UX Principles Checklist](./ux-principles-checklist.md)

---

**Last Updated**: 2025-01-XX  
**Maintained By**: Design System Team
```

---

## Documentation Requirements

### Required Sections

1. **Overview**: Brief description of purpose and usage
2. **API**: Complete prop documentation
3. **Examples**: Usage examples for common cases
4. **Accessibility**: Accessibility features and requirements
5. **Migration Notes**: If replacing existing component

### Optional Sections

- **Variants**: If component has multiple variants
- **Sizes**: If component has size options
- **Implementation Details**: Technical details for developers
- **Testing**: Test coverage and examples
- **Related Documentation**: Links to related docs

---

## Documentation Standards

### Code Examples

- Use TypeScript for React examples
- Include complete, runnable examples
- Show common use cases
- Include edge cases when relevant

### Accessibility Documentation

- Document keyboard navigation
- Document screen reader support
- Document color contrast requirements
- Include accessibility testing notes

### Migration Documentation

- Document breaking changes
- Provide step-by-step migration guide
- Include before/after examples
- Note any deprecation timelines

---

## Resources

- [Component Structure Template](./component-structure-template.md) - Component standards
- [Code Review Checklist](./code-review-checklist.md) - Review criteria
- [UX Principles Checklist](./ux-principles-checklist.md) - Quality standards

---

**Last Updated**: 2025-01-XX  
**Maintained By**: Design System Team

