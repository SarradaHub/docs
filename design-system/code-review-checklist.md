# Code Review Checklist - Phase 3 Standards

**Purpose**: Comprehensive checklist for reviewing PRs against Phase 3 standardization requirements

**Related Documents**:
- [Phase 3 Standardization](./phase-3-standardization.md) - Overall standards
- [Component Structure Template](./component-structure-template.md) - Component standards
- [Grid System Guide](./grid-system-guide.md) - Layout standards
- [UX Principles Checklist](./ux-principles-checklist.md) - Quality standards

---

## How to Use This Checklist

1. **For every PR**: Review against all relevant sections
2. **Check all boxes**: Mark items as complete or note exceptions
3. **Document exceptions**: If an item doesn't apply, note why
4. **Block on critical items**: Must-have items block PR approval
5. **Suggest improvements**: Should-have items can be follow-up tickets

---

## Component Structure

### File Structure
- [ ] Follows standard file structure (ComponentName/ComponentName.tsx or _component_name.html.erb)
- [ ] Uses TypeScript for React components
- [ ] Co-locates tests and stories with components (if applicable)
- [ ] Uses index.ts for clean exports (React components)

### Component Code
- [ ] Uses design system tokens for all styling
- [ ] Supports `className` prop for composition
- [ ] Includes accessibility props (aria-label, role, etc.) when needed
- [ ] Uses TypeScript interfaces for props (React components)
- [ ] Follows design system prop naming conventions (`variant`, `size`, not `type`, `scale`)
- [ ] Uses `forwardRef` when component needs ref forwarding (React)
- [ ] Sets `displayName` for better debugging (React)

### CSS Methodology
- [ ] Uses Tailwind utility classes from design system
- [ ] Uses CSS variables for design tokens (colors, spacing, typography)
- [ ] No inline styles (except dynamic values)
- [ ] No custom CSS files unless absolutely necessary
- [ ] Uses `cn()` utility for conditional class names

### Prop API
- [ ] Uses `variant` not `type` or `style`
- [ ] Uses `size` not `scale` or `dimension`
- [ ] Uses `disabled` not `isDisabled`
- [ ] Always supports `className` prop
- [ ] Includes accessibility props when needed
- [ ] Provides sensible defaults for optional props

### Composition
- [ ] Prefers composition over configuration
- [ ] Uses compound components for complex UI (if applicable)
- [ ] Allows `children` prop for flexibility (if applicable)
- [ ] Keeps components focused and small
- [ ] Composes complex components from simple ones

---

## Layout & Grid

### Grid System
- [ ] Uses 12-column grid system for multi-column layouts
- [ ] Elements align to grid columns
- [ ] Uses max-width container (`max-w-7xl mx-auto`)
- [ ] Applies consistent gutters using design system spacing tokens
- [ ] Uses responsive breakpoints correctly (`md:`, `lg:`, etc.)

### Spacing
- [ ] Uses design system spacing tokens (no hardcoded values)
- [ ] Consistent spacing creates visual rhythm
- [ ] No arbitrary spacing values
- [ ] Proper gap classes for grid layouts (`gap-4 md:gap-6`)

### Responsive Design
- [ ] Mobile-first approach (starts with `grid-cols-1`)
- [ ] Breakpoints used consistently
- [ ] Column spans adjust appropriately at breakpoints
- [ ] Layout works at all screen sizes

---

## UX Principles

### Clarity & Hierarchy
- [ ] Component/page purpose is immediately clear
- [ ] Single primary action per view
- [ ] Visual hierarchy uses size, color, position effectively
- [ ] Adequate white space (uses design system spacing)
- [ ] Logical grouping of related information
- [ ] Most important information is most prominent

### User Journey & Flow
- [ ] Intuitive user path (users can complete tasks without confusion)
- [ ] Reduced cognitive load (information in digestible chunks)
- [ ] Clear next steps (users know what to do next)
- [ ] Minimal steps to goal (fewest clicks/actions)
- [ ] Escape hatches available (back/cancel options)
- [ ] Forms optimized (only necessary fields)
- [ ] Multi-step processes broken into logical steps

### Accessibility (a11y)
- [ ] All interactive elements keyboard navigable
- [ ] Focus indicators visible (not just `outline: none`)
- [ ] Semantic HTML used (button, not div with onClick)
- [ ] ARIA labels on interactive elements without visible text
- [ ] ARIA roles on complex components
- [ ] Form labels associated with inputs (`htmlFor`/`id`)
- [ ] Error messages associated with form fields
- [ ] Color contrast meets WCAG AA (4.5:1 normal, 3:1 large)
- [ ] Information not conveyed by color alone
- [ ] Screen reader friendly (tested or verified)

### Consistency & Feedback
- [ ] Uses design system components (no custom when equivalent exists)
- [ ] Consistent patterns for similar actions
- [ ] Immediate feedback for interactions (hover, click, focus)
- [ ] Loading states for actions >500ms
- [ ] Clear, actionable error messages
- [ ] Success confirmations for significant actions
- [ ] Button styles consistent for similar actions
- [ ] Hover/active states consistent

---

## Code Quality

### Design System Usage
- [ ] No duplicate code (uses design system components)
- [ ] No hardcoded values (colors, spacing, typography)
- [ ] Imports from design system package correctly
- [ ] Uses design system tokens for all styling

### Code Organization
- [ ] No duplicate components
- [ ] Code is maintainable and readable
- [ ] Follows project structure conventions
- [ ] Proper separation of concerns

### Testing
- [ ] Tests included (if applicable)
- [ ] Accessibility tests included (if applicable)
- [ ] Tests cover main functionality
- [ ] Edge cases handled in tests

### Documentation
- [ ] Component API documented (props, examples)
- [ ] Usage examples provided
- [ ] Accessibility notes included
- [ ] Migration notes included (if replacing existing component)
- [ ] JSDoc comments on complex functions (if applicable)

---

## Performance

### Loading Performance
- [ ] Fast initial load
- [ ] Code splitting used for large components (if applicable)
- [ ] Optimized assets (images, fonts)

### Runtime Performance
- [ ] Smooth interactions (60fps animations)
- [ ] No layout shifts during load (CLS)
- [ ] Efficient re-renders (React components optimized with memo, useMemo where needed)

---

## Mobile & Responsive

### Mobile Optimization
- [ ] Mobile-first approach
- [ ] Touch targets adequate (at least 44x44 pixels)
- [ ] Content prioritization (most important visible without scrolling)
- [ ] Fast load times on mobile networks

### Responsive Behavior
- [ ] Breakpoints used consistently
- [ ] Layout adapts gracefully at all screen sizes
- [ ] Text readable at all screen sizes
- [ ] Images responsive
- [ ] Navigation adapts (mobile menu/hamburger for small screens)

---

## Critical Items (Blockers)

These items **must** be addressed before PR approval:

- [ ] Uses design system components
- [ ] Keyboard navigable
- [ ] Color contrast meets WCAG AA
- [ ] Clear primary action
- [ ] Loading/error states handled
- [ ] Mobile responsive
- [ ] No hardcoded values (colors, spacing)
- [ ] Uses 12-column grid for multi-column layouts
- [ ] Semantic HTML used
- [ ] Form labels associated

---

## Should-Have Items (Important)

These items are important but can be follow-up tickets:

- [ ] Consistent with design patterns
- [ ] Clear visual hierarchy
- [ ] Immediate feedback for interactions
- [ ] Accessible to screen readers
- [ ] Optimized for performance
- [ ] Tests included
- [ ] Documentation complete

---

## Nice-to-Have Items (Enhancements)

These items are enhancements that can be added later:

- [ ] Micro-animations
- [ ] Advanced keyboard shortcuts
- [ ] Skeleton loading states
- [ ] Advanced accessibility features
- [ ] Storybook stories
- [ ] Comprehensive test coverage

---

## Review Notes

_Use this space to document any exceptions, special considerations, or additional context:_

_______________________________________________________________________
_______________________________________________________________________
_______________________________________________________________________

---

## Review Checklist Summary

**Reviewer**: _______________________  
**Date**: _______________________  
**PR**: _______________________  

**Status**:
- [ ] ✅ Approved - All critical items met
- [ ] ⏳ Changes Requested - Critical items need attention
- [ ] 📋 Follow-up Needed - Should-have items for future PR

**Critical Items Status**: ___ / ___ complete  
**Overall Compliance**: ___%  

---

**Last Updated**: 2025-01-XX  
**Maintained By**: Design System Team

