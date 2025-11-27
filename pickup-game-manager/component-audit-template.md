# Component Audit Template

Use this template to systematically audit each view in `pickup-game-manager` and identify where design system components are missing or incorrectly implemented.

**Reference Documents:**
- [UX Principles Checklist](../design-system/ux-principles-checklist.md)
- [UI Kit Implementation Recommendation](../design-system/ui-kit-implementation-recommendation.md)
- [View Conversion Guide](./view-conversion-guide.md)

---

## View Metadata

**View Path**: `app/views/_________________/_________________.html.erb`

**Controller**: `_________________Controller`

**Purpose**: 
- [ ] Index/List view
- [ ] Show/Detail view
- [ ] New/Create form
- [ ] Edit/Update form
- [ ] Partial component
- [ ] Dashboard/Overview

**User Flow**: 
- Primary action: _________________
- Secondary actions: _________________
- Navigation path: _________________

**Traffic/Usage**: 
- [ ] High (daily use)
- [ ] Medium (weekly use)
- [ ] Low (occasional use)

---

## Component Inventory Checklist

### Buttons & Links

#### Primary Actions
- [ ] Uses design system primary button classes
- [ ] Has proper hover states (`hover:bg-primary-700`)
- [ ] Has proper focus states (`focus:ring-2 focus:ring-primary-500`)
- [ ] Has proper disabled states
- [ ] Uses correct size classes (`px-4 py-2 text-sm` for md)
- [ ] Accessible (keyboard navigable, proper ARIA)

**Current Implementation:**
```erb
<!-- Paste current button/link code here -->
```

**Design System Standard:**
```erb
<!-- Should use: inline-flex items-center justify-center rounded-lg font-medium bg-primary-600 text-white hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 transition-colors px-4 py-2 text-sm -->
```

#### Secondary Actions
- [ ] Uses design system secondary button classes
- [ ] Proper variant styling (`bg-neutral-200 text-neutral-900 hover:bg-neutral-300`)
- [ ] Consistent with primary button structure

**Current Implementation:**
```erb
<!-- Paste current secondary button code here -->
```

#### Text Links
- [ ] Uses text link variant (`text-primary-600 hover:text-primary-800`)
- [ ] Has underline or clear visual indication
- [ ] Proper focus states

**Current Implementation:**
```erb
<!-- Paste current link code here -->
```

#### Danger Actions
- [ ] Uses danger variant (`bg-error-600 hover:bg-error-700`)
- [ ] Only used for destructive actions

**Current Implementation:**
```erb
<!-- Paste current danger button code here -->
```

---

### Cards

#### Card Components
- [ ] Uses design system Card classes
- [ ] Proper variant (default/outlined/elevated)
- [ ] Proper padding (`p-4`, `p-6`, or `p-8`)
- [ ] Rounded corners (`rounded-lg`)
- [ ] Proper border styling (if outlined variant)
- [ ] Proper shadow (if elevated variant)

**Current Implementation:**
```erb
<!-- Paste current card code here -->
```

**Design System Standard:**
```erb
<!-- Default: rounded-lg bg-white dark:bg-neutral-800 p-6 -->
<!-- Outlined: rounded-lg bg-white dark:bg-neutral-800 border border-neutral-200 dark:border-neutral-700 p-6 -->
<!-- Elevated: rounded-lg bg-white dark:bg-neutral-800 shadow-md p-6 -->
```

#### Card Structure
- [ ] Uses CardHeader for titles (if applicable)
- [ ] Uses CardContent for main content
- [ ] Uses CardFooter for actions (if applicable)
- [ ] Proper semantic HTML structure

**Current Implementation:**
```erb
<!-- Paste current card structure here -->
```

---

### Alerts & Notifications

#### Success Alerts
- [ ] Uses design system Alert classes
- [ ] Proper variant (`bg-success-50 text-success-800 border-success-200`)
- [ ] Has icon (if applicable)
- [ ] Proper ARIA attributes (`role="alert"`, `aria-live="polite"`)

**Current Implementation:**
```erb
<!-- Paste current success alert code here -->
```

**Design System Standard:**
```erb
<!-- flex gap-3 p-4 rounded-lg border bg-success-50 text-success-800 border-success-200 -->
```

#### Error Alerts
- [ ] Uses error variant (`bg-error-50 text-error-800 border-error-200`)
- [ ] Shows validation errors properly
- [ ] Associated with form fields (if applicable)

**Current Implementation:**
```erb
<!-- Paste current error alert code here -->
```

#### Warning Alerts
- [ ] Uses warning variant (`bg-warning-50 text-warning-800 border-warning-200`)

**Current Implementation:**
```erb
<!-- Paste current warning alert code here -->
```

#### Info Alerts
- [ ] Uses info variant (`bg-info-50 text-info-800 border-info-200`)

**Current Implementation:**
```erb
<!-- Paste current info alert code here -->
```

---

### Forms

#### Input Fields
- [ ] Uses design system Input classes
- [ ] Proper label association (`for` and `id` attributes)
- [ ] Error state styling (`border-error-500 focus:ring-error-500`)
- [ ] Focus states (`focus:ring-2 focus:ring-primary-500`)
- [ ] Disabled states
- [ ] Helper text styling (if applicable)
- [ ] Required field indicators (`*` with `text-error-500`)

**Current Implementation:**
```erb
<!-- Paste current input code here -->
```

**Design System Standard:**
```erb
<!-- Input: w-full px-4 py-2 text-base text-neutral-900 bg-white border border-neutral-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent -->
<!-- Label: block text-sm font-medium text-neutral-700 mb-1 -->
<!-- Error: mt-1 text-sm text-error-600 -->
```

#### Textarea Fields
- [ ] Uses design system Textarea classes
- [ ] Same structure as Input (label, error handling, focus states)
- [ ] Proper `resize-y` class

**Current Implementation:**
```erb
<!-- Paste current textarea code here -->
```

#### Select Fields
- [ ] Uses design system Select classes
- [ ] Same structure as Input
- [ ] Proper option styling

**Current Implementation:**
```erb
<!-- Paste current select code here -->
```

#### Checkboxes
- [ ] Uses design system Checkbox classes
- [ ] Proper label association
- [ ] Custom checkbox styling (`h-5 w-5 appearance-none rounded border-2`)
- [ ] Check icon visible when checked

**Current Implementation:**
```erb
<!-- Paste current checkbox code here -->
```

#### Radio Buttons
- [ ] Uses design system Radio classes (if available)
- [ ] Proper grouping with `fieldset`/`legend`
- [ ] Proper label association

**Current Implementation:**
```erb
<!-- Paste current radio code here -->
```

#### Form Structure
- [ ] Uses `fieldset`/`legend` for related fields
- [ ] Proper form validation display
- [ ] Error messages associated with fields (`aria-describedby`)
- [ ] Form submission button uses design system Button

**Current Implementation:**
```erb
<!-- Paste current form structure here -->
```

---

### Lists & Tables

#### List Components
- [ ] Uses design system List classes (if available)
- [ ] Proper spacing between items
- [ ] Consistent styling

**Current Implementation:**
```erb
<!-- Paste current list code here -->
```

#### Table Components
- [ ] Uses design system Table classes (if available)
- [ ] Proper header styling
- [ ] Alternating row colors (if applicable)
- [ ] Responsive design (mobile-friendly)

**Current Implementation:**
```erb
<!-- Paste current table code here -->
```

---

### Navigation Elements

#### Navigation Links
- [ ] Uses design system navigation patterns
- [ ] Active state styling
- [ ] Proper hover states

**Current Implementation:**
```erb
<!-- Paste current navigation code here -->
```

---

## Current Implementation Analysis

### Custom Classes vs Design System

**Custom Classes Found:**
```
<!-- List all custom classes that don't match design system -->
```

**Should Be Replaced With:**
```
<!-- List design system equivalents -->
```

### Inline Styles

- [ ] No inline styles used
- [ ] All styling via Tailwind classes
- [ ] CSS variables used for colors (if applicable)

**Inline Styles Found:**
```erb
<!-- List any inline styles -->
```

### Missing Accessibility Attributes

- [ ] All interactive elements have proper ARIA labels
- [ ] Form fields have `aria-describedby` for errors
- [ ] Form fields have `aria-invalid` when errors exist
- [ ] Buttons have proper `aria-label` if icon-only
- [ ] Alerts have `role="alert"` and `aria-live`
- [ ] Images have `alt` text
- [ ] Focus indicators visible
- [ ] Keyboard navigation works

**Missing Attributes:**
```
<!-- List missing accessibility attributes -->
```

---

## Design System Compliance Scoring

### Component Usage
- **Score**: ___ / 10
- **Notes**: _________________

### Class Consistency
- **Score**: ___ / 10
- **Notes**: _________________

### Accessibility
- **Score**: ___ / 10
- **Notes**: _________________

### Responsive Design
- **Score**: ___ / 10
- **Notes**: _________________

### UX Principles Compliance
- **Score**: ___ / 10
- **Notes**: _________________

**Overall Compliance**: ___ / 50

**Priority Level**: 
- [ ] P0 (Critical - needs immediate attention)
- [ ] P1 (High - should be updated soon)
- [ ] P2 (Medium - can be updated later)
- [ ] P3 (Low - nice to have)

---

## UX Principles Compliance

Reference: [UX Principles Checklist](../design-system/ux-principles-checklist.md)

### Clarity & Simplicity
- [ ] Purpose is immediately clear
- [ ] Single primary action
- [ ] Adequate white space
- [ ] Logical grouping

### Visual Hierarchy
- [ ] Most important information is most prominent
- [ ] Primary actions stand out
- [ ] Clear heading structure

### Consistency
- [ ] Uses design system components
- [ ] Consistent button styles
- [ ] Consistent spacing (8px base unit)
- [ ] Consistent colors (design tokens)
- [ ] Consistent typography

### User Journey & Flow
- [ ] Intuitive user path
- [ ] Clear next steps
- [ ] Minimal steps to goal
- [ ] Escape hatches available

### Feedback & Response
- [ ] Immediate visual feedback
- [ ] Loading states (if applicable)
- [ ] Success confirmations
- [ ] Clear error messages

### Accessibility
- [ ] Fully keyboard navigable
- [ ] Focus indicators visible
- [ ] Semantic HTML
- [ ] ARIA labels where needed
- [ ] Color contrast meets WCAG AA

### Mobile & Responsive
- [ ] Mobile-first approach
- [ ] Touch targets adequate (44x44px)
- [ ] Layout adapts gracefully
- [ ] Text readable on mobile

---

## Action Items

### Immediate Fixes (Blockers)
1. _________________
2. _________________
3. _________________

### High Priority Updates
1. _________________
2. _________________
3. _________________

### Medium Priority Updates
1. _________________
2. _________________
3. _________________

### Nice to Have
1. _________________
2. _________________
3. _________________

---

## Notes

_Use this space to document any exceptions, special considerations, or additional context:_

_______________________________________________________________________
_______________________________________________________________________
_______________________________________________________________________

---

**Audited By**: _________________

**Date**: _________________

**Next Review Date**: _________________

