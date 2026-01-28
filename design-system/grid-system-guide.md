# 12-Column Grid System Guide

**Purpose**: Standardize layout structure across all pages using a consistent 12-column grid system

**Related Documents**:
- [Grid Examples](./grid-examples.md) - Common layout patterns
- [Phase 3 Standardization](./phase-3-standardization.md) - Overall Phase 3 standards
- [Component Structure Template](./component-structure-template.md) - Component standards

---

## Grid Specifications

### Core Specifications

- **Columns**: 12 columns
- **Gutters**: Consistent spacing between columns (using design system spacing tokens)
- **Max-width container**: Content doesn't stretch too wide on large screens
- **Breakpoints**: Use design system breakpoints
  - `sm`: 640px
  - `md`: 768px
  - `lg`: 1024px
  - `xl`: 1280px
  - `2xl`: 1536px

### Container Pattern

All pages should use a max-width container to prevent content from stretching too wide:

```tsx
// React
<div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
  {/* Page content */}
</div>
```

```erb
<!-- Rails -->
<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
  <%= yield %>
</div>
```

### Grid Pattern

Use Tailwind's grid system with 12 columns:

```tsx
// React
<div className="grid grid-cols-1 md:grid-cols-12 gap-4 md:gap-6">
  {/* Grid items */}
</div>
```

```erb
<!-- Rails -->
<div class="grid grid-cols-1 md:grid-cols-12 gap-4 md:gap-6">
  <!-- Grid items -->
</div>
```

---

## Implementation

### For React Projects

#### Basic Grid Container

```tsx
import { PropsWithChildren } from 'react';

interface GridContainerProps extends PropsWithChildren {
  className?: string;
}

export function GridContainer({ children, className }: GridContainerProps) {
  return (
    <div className={cn(
      'max-w-7xl mx-auto px-4 sm:px-6 lg:px-8',
      className
    )}>
      {children}
    </div>
  );
}
```

#### Grid Layout Component

```tsx
import { PropsWithChildren } from 'react';
import { cn } from '@sarradahub/design-system/utils';

interface GridLayoutProps extends PropsWithChildren {
  className?: string;
  gap?: 'sm' | 'md' | 'lg';
}

export function GridLayout({ 
  children, 
  className,
  gap = 'md' 
}: GridLayoutProps) {
  const gapClasses = {
    sm: 'gap-2 md:gap-4',
    md: 'gap-4 md:gap-6',
    lg: 'gap-6 md:gap-8',
  };

  return (
    <div className={cn(
      'grid grid-cols-1 md:grid-cols-12',
      gapClasses[gap],
      className
    )}>
      {children}
    </div>
  );
}
```

#### Grid Item Component

```tsx
import { PropsWithChildren } from 'react';
import { cn } from '@sarradahub/design-system/utils';

interface GridItemProps extends PropsWithChildren {
  span?: 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12;
  spanMd?: 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12;
  spanLg?: 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12;
  className?: string;
}

export function GridItem({ 
  children, 
  span = 12,
  spanMd,
  spanLg,
  className 
}: GridItemProps) {
  return (
    <div className={cn(
      `col-span-${span}`,
      spanMd && `md:col-span-${spanMd}`,
      spanLg && `lg:col-span-${spanLg}`,
      className
    )}>
      {children}
    </div>
  );
}
```

**Note**: Tailwind's JIT compiler may not recognize dynamic class names. Use explicit classes or a mapping:

```tsx
const spanClasses = {
  1: 'col-span-1',
  2: 'col-span-2',
  3: 'col-span-3',
  4: 'col-span-4',
  5: 'col-span-5',
  6: 'col-span-6',
  7: 'col-span-7',
  8: 'col-span-8',
  9: 'col-span-9',
  10: 'col-span-10',
  11: 'col-span-11',
  12: 'col-span-12',
};

const mdSpanClasses = {
  1: 'md:col-span-1',
  2: 'md:col-span-2',
  // ... etc
};
```

### For Rails Projects

#### Layout Helper

```ruby
# app/helpers/grid_helper.rb
module GridHelper
  def grid_container(options = {}, &block)
    classes = [
      'max-w-7xl mx-auto px-4 sm:px-6 lg:px-8',
      options[:class]
    ].compact.join(' ')
    
    content_tag(:div, class: classes, &block)
  end

  def grid_layout(options = {}, &block)
    gap = options[:gap] || 'md'
    gap_classes = {
      'sm' => 'gap-2 md:gap-4',
      'md' => 'gap-4 md:gap-6',
      'lg' => 'gap-6 md:gap-8'
    }
    
    classes = [
      'grid grid-cols-1 md:grid-cols-12',
      gap_classes[gap],
      options[:class]
    ].compact.join(' ')
    
    content_tag(:div, class: classes, &block)
  end

  def grid_item(span: 12, span_md: nil, span_lg: nil, options: {}, &block)
    span_classes = {
      1 => 'col-span-1', 2 => 'col-span-2', 3 => 'col-span-3',
      4 => 'col-span-4', 5 => 'col-span-5', 6 => 'col-span-6',
      7 => 'col-span-7', 8 => 'col-span-8', 9 => 'col-span-9',
      10 => 'col-span-10', 11 => 'col-span-11', 12 => 'col-span-12'
    }
    
    md_span_classes = span_md ? {
      1 => 'md:col-span-1', 2 => 'md:col-span-2', 3 => 'md:col-span-3',
      4 => 'md:col-span-4', 5 => 'md:col-span-5', 6 => 'md:col-span-6',
      7 => 'md:col-span-7', 8 => 'md:col-span-8', 9 => 'md:col-span-9',
      10 => 'md:col-span-10', 11 => 'md:col-span-11', 12 => 'md:col-span-12'
    }[span_md] : nil
    
    lg_span_classes = span_lg ? {
      1 => 'lg:col-span-1', 2 => 'lg:col-span-2', 3 => 'lg:col-span-3',
      4 => 'lg:col-span-4', 5 => 'lg:col-span-5', 6 => 'lg:col-span-6',
      7 => 'lg:col-span-7', 8 => 'lg:col-span-8', 9 => 'lg:col-span-9',
      10 => 'lg:col-span-10', 11 => 'lg:col-span-11', 12 => 'lg:col-span-12'
    }[span_lg] : nil
    
    classes = [
      span_classes[span],
      md_span_classes,
      lg_span_classes,
      options[:class]
    ].compact.join(' ')
    
    content_tag(:div, class: classes, &block)
  end
end
```

#### Usage in ERB

```erb
<%= grid_container do %>
  <%= grid_layout gap: 'md' do %>
    <%= grid_item span: 12, span_md: 3 do %>
      <!-- Sidebar content -->
    <% end %>
    
    <%= grid_item span: 12, span_md: 9 do %>
      <!-- Main content -->
    <% end %>
  <% end %>
<% end %>
```

---

## Grid Usage Rules

### Always Use Grid For

- ✅ **Multi-column layouts**: Don't use flexbox for column-based layouts
- ✅ **Page layouts**: All pages should use grid
- ✅ **Card grids**: Use grid for responsive card layouts
- ✅ **Form layouts**: Multi-column forms should use grid

### Grid Alignment

- ✅ **Elements align to grid**: All major elements align to grid columns
- ✅ **Consistent spacing**: Use design system spacing scale
- ✅ **Visual rhythm**: Consistent spacing creates visual rhythm
- ✅ **No arbitrary spacing**: Avoid random spacing values

### Responsive Behavior

- ✅ **Mobile-first**: Start with single column (`grid-cols-1`)
- ✅ **Breakpoints**: Use `md:` prefix for tablet+ layouts
- ✅ **Column spans**: Adjust column spans at different breakpoints
- ✅ **Gutters**: Increase gutters on larger screens

---

## Spacing Tokens

Use design system spacing tokens for gutters:

- **Small gap**: `gap-2 md:gap-4` (8px mobile, 16px desktop)
- **Medium gap**: `gap-4 md:gap-6` (16px mobile, 24px desktop)
- **Large gap**: `gap-6 md:gap-8` (24px mobile, 32px desktop)

These correspond to design system spacing scale:
- `2` = 0.5rem (8px)
- `4` = 1rem (16px)
- `6` = 1.5rem (24px)
- `8` = 2rem (32px)

---

## Max-Width Container

Use `max-w-7xl` (1280px) for main content containers:

```tsx
<div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
```

This prevents content from stretching too wide on large screens while maintaining readability.

**Alternative max-widths** (use sparingly):
- `max-w-5xl` (1024px) - For narrower content
- `max-w-6xl` (1152px) - Medium width
- `max-w-7xl` (1280px) - Standard (recommended)
- `max-w-full` - Full width (use only when necessary)

---

## Common Patterns

See [Grid Examples](./grid-examples.md) for detailed implementation examples of:

- Two-column layout (sidebar + content)
- Three-column layout
- Card grid layouts
- Responsive grid patterns
- Form layouts

---

## Migration Guide

### Updating Existing Pages

1. **Wrap page content** in max-width container
2. **Replace flexbox layouts** with grid where appropriate
3. **Apply grid classes** to main content areas
4. **Update column spans** for responsive behavior
5. **Verify spacing** uses design system tokens

### Example Migration

**Before**:
```tsx
<div className="flex">
  <aside className="w-1/4">Sidebar</aside>
  <main className="flex-1">Content</main>
</div>
```

**After**:
```tsx
<div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
  <div className="grid grid-cols-1 md:grid-cols-12 gap-6">
    <aside className="md:col-span-3">Sidebar</aside>
    <main className="md:col-span-9">Content</main>
  </div>
</div>
```

---

## Resources

- [Grid Examples](./grid-examples.md) - Common layout patterns
- [Component Structure Template](./component-structure-template.md) - Component standards
- [Phase 3 Standardization](./phase-3-standardization.md) - Overall standards
- [Design System Tokens](../../platform/design-system/src/tokens) - Spacing and breakpoint tokens

---

**Last Updated**: 2025-01-XX  
**Maintained By**: Design System Team

