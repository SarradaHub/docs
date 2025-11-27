# Phase 1 Audit: pickup-game-manager

**Repository**: pickup-game-manager  
**Tech Stack**: Rails 8, ERB views, Tailwind CSS Rails  
**Date**: 2025-01-XX  
**Auditor**: System

---

## Repository Information

- **Repository Name**: pickup-game-manager
- **Tech Stack**: Rails 8.1.1, PostgreSQL, Turbo, Stimulus
- **Package Manager**: Bundler (Ruby gems)
- **Build Tool**: Rails asset pipeline (Propshaft)
- **Styling Approach**: Tailwind CSS + Custom CSS (dashboard.css)
- **Current Design System Usage**: Partial (Tailwind config extends design system, but CSS variables are manually duplicated)

---

## 1. Component Inventory

### 1.1 Core UI Components

#### Buttons
- **Location**: Used in ERB views with custom CSS classes
- **Variants Found**:
  - `.action-btn.primary` - Primary action buttons (dashboard)
  - `.action-btn.secondary` - Secondary action buttons (dashboard)
  - `.btn-primary` - Form submit buttons
  - `.btn-secondary` - Secondary form buttons
  - `.btn-small` - Small buttons for cards/links
  - `.btn-small.secondary` - Small secondary buttons
- **States**: Hover states defined in CSS
- **Sizes**: Small and default
- **Notes**: 
  - Custom CSS classes, not using design system Button component
  - Hardcoded gradient colors (#667eea, #764ba2)
  - Inline styles in some views (`style="color: green"`)

#### Form Inputs
- **Location**: `app/views/**/_form.html.erb`
- **Current Implementation**: Basic Rails form helpers
- **Features**:
  - Basic labels with `style="display: block"`
  - No error styling beyond default Rails
  - No helper text
  - No validation feedback styling
- **Form Components Found**:
  - Text inputs (`text_field`)
  - Date inputs (`date_field`)
  - Textarea (`textarea`)
  - Checkboxes (`check_box_tag`)
  - Select (not seen, but likely available)
- **Notes**: 
  - Very basic styling
  - Error messages use inline `style="color: red"`
  - No consistent form component structure

#### Cards/Containers
- **Location**: Dashboard and various views
- **Variants Found**:
  - `.stat-card` - Statistics cards
  - `.activity-card` - Activity list cards
  - `.match-card` - Match display cards
  - `.equilibrium-card` - Equilibrium analysis card
  - `.debtor-item` - Debtor list items
  - `.summary-card` - Summary cards
- **Notes**: Custom CSS, not using design system Card component

#### Navigation Components
- **Location**: Not found (likely minimal navigation)
- **Features**: None identified
- **Notes**: No navbar/sidebar components found

#### Feedback Components
- **Location**: Views use basic Rails flash messages
- **Current Implementation**:
  - Flash notices: `<p style="color: green"><%= notice %></p>`
  - Error messages: Inline red styling
- **Notes**: 
  - No modal/dialog components
  - No toast/notification system
  - No loading spinners
  - Basic error display

#### Data Display Components
- **Location**: Dashboard and index views
- **Components Found**:
  - Lists (activity lists, breakdown lists)
  - Tables (not explicitly styled, likely default HTML)
  - Badges (`.status-badge` with `.paid` and `.pending` variants)
- **Notes**: Custom CSS for badges, no design system components

#### Layout Components
- **Location**: `app/views/layouts/application.html.erb`
- **Grid System**: CSS Grid used in dashboard.css
- **Container**: `.dashboard` with `max-width: 1200px`
- **Notes**: Custom layout, not using design system grid

### 1.2 Component Patterns

- **Composition Pattern**: Not applicable (ERB views)
- **Prop Naming Conventions**: CSS class-based
- **Styling Methodology**: Tailwind CSS + Custom CSS (dashboard.css)
- **Component Structure**: ERB partials (`_form.html.erb`, `_payment.html.erb`, etc.)

---

## 2. Page/View Audit

### 2.1 Route/Page Inventory

**ERB Views Found** (35 total):

#### Dashboard
- `app/views/dashboard/index.html.erb` - Main dashboard with financial overview, charts, stats

#### Payments
- `app/views/payments/index.html.erb` - Payment list
- `app/views/payments/show.html.erb` - Payment detail
- `app/views/payments/new.html.erb` - New payment form
- `app/views/payments/edit.html.erb` - Edit payment form
- `app/views/payments/_form.html.erb` - Payment form partial
- `app/views/payments/_payment.html.erb` - Payment display partial

#### Matches
- `app/views/matches/index.html.erb` - Match list
- `app/views/matches/show.html.erb` - Match detail
- `app/views/matches/new.html.erb` - New match form
- `app/views/matches/edit.html.erb` - Edit match form
- `app/views/matches/_form.html.erb` - Match form partial
- `app/views/matches/_match.html.erb` - Match display partial

#### Athletes
- `app/views/athletes/index.html.erb` - Athlete list
- `app/views/athletes/show.html.erb` - Athlete detail
- `app/views/athletes/new.html.erb` - New athlete form
- `app/views/athletes/edit.html.erb` - Edit athlete form
- `app/views/athletes/_form.html.erb` - Athlete form partial
- `app/views/athletes/_athlete.html.erb` - Athlete display partial

#### Expenses
- `app/views/expenses/index.html.erb` - Expense list
- `app/views/expenses/show.html.erb` - Expense detail
- `app/views/expenses/new.html.erb` - New expense form
- `app/views/expenses/edit.html.erb` - Edit expense form
- `app/views/expenses/_form.html.erb` - Expense form partial
- `app/views/expenses/_expense.html.erb` - Expense display partial

#### Incomes
- `app/views/incomes/index.html.erb` - Income list
- `app/views/incomes/show.html.erb` - Income detail
- `app/views/incomes/new.html.erb` - New income form
- `app/views/incomes/edit.html.erb` - Edit income form
- `app/views/incomes/_form.html.erb` - Income form partial
- `app/views/incomes/_income.html.erb` - Income display partial

#### Layouts
- `app/views/layouts/application.html.erb` - Main application layout
- `app/views/layouts/mailer.html.erb` - Email layout
- `app/views/layouts/mailer.text.erb` - Plain text email layout

### 2.2 Layout Structure

- **Main Layout**: `app/views/layouts/application.html.erb`
  - Structure: Basic HTML with `<div class="app-container">`
  - No header/navbar
  - No footer
  - Minimal structure

- **Navigation Structure**: None found
- **Content Wrapper**: `.app-container` with minimal styling

---

## 3. Design Tokens & Styling

### 3.1 Color System

- **Primary Colors**: 
  - Defined in `application.css` as CSS variables
  - Values match design system (blue palette: #eff6ff to #172554)
  - **Issue**: Manually duplicated, not imported from design system

- **Secondary Colors**: 
  - Defined in CSS variables (purple palette)
  - Values match design system

- **Semantic Colors**: 
  - Success: #22c55e, #16a34a
  - Warning: #eab308, #ca8a04
  - Error: #ef4444, #dc2626
  - Info: #3b82f6, #2563eb

- **Neutral/Gray Scale**: 
  - Defined in CSS variables (#f9fafb to #030712)

- **Color Usage Patterns**:
  - ✅ CSS variables defined
  - ❌ Hardcoded hex values in dashboard.css (#667eea, #764ba2, #2d3748, etc.)
  - ❌ Inline styles in views (`style="color: green"`, `style="color: red"`)

### 3.2 Typography

- **Font Family**: 
  - Dashboard: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`
  - Application: Not explicitly set (browser default)
  - **Issue**: Not using design system font (Inter)

- **Font Sizes**: 
  - Custom sizes in dashboard.css (2.5rem, 1.5rem, 1.25rem, etc.)
  - Not using design system typography scale

- **Font Weights**: Custom (400, 500, 600, 700)

### 3.3 Spacing System

- **Spacing Scale**: 
  - CSS variables defined (0 to 32rem)
  - Matches design system
  - **Issue**: Not consistently used (hardcoded px values in dashboard.css)

### 3.4 Other Design Tokens

- **Border Radius**: 
  - CSS variables defined (none to full)
  - Custom values in dashboard.css (12px, 16px, 8px, 6px)

- **Shadows/Elevation**: 
  - CSS variables defined for light/dark mode
  - Custom shadows in dashboard.css

- **Breakpoints**: 
  - Custom media queries in dashboard.css (@media max-width: 768px, 480px)
  - Not using design system breakpoints

---

## 4. Gap Analysis

### 4.1 Design System Alignment

#### Buttons
- ❌ **Does NOT match design system**
- Missing: Design system Button component (React-based, not applicable)
- Missing: Consistent button API
- Issue: Hardcoded colors, custom gradients

#### Form Inputs
- ❌ **Does NOT match design system**
- Missing: Design system Input component styling
- Missing: Error states, helper text, validation feedback
- Issue: Basic Rails form helpers with minimal styling

#### Cards
- ❌ **Does NOT match design system**
- Missing: Design system Card component structure
- Issue: Custom CSS classes, hardcoded colors

#### Modals
- ❌ **Does NOT exist**
- No modal/dialog components found

#### Navigation
- ❌ **Does NOT exist**
- No navigation components

### 4.2 Missing Components

Components in design system but not in this repo:
- Modal/Dialog
- Alert/Toast
- Loading Spinner
- Table (styled)
- Navbar
- Sidebar
- Form components (Input, Select, Checkbox, Radio with design system styling)

### 4.3 Custom Components

Components that exist in this repo but not in design system:
- Dashboard-specific components (stat cards, chart components, equilibrium calculator)
- **Decision needed**: These are domain-specific and may not need to be in design system

---

## 5. Accessibility Audit

### 5.1 Keyboard Navigation

- ⚠️ **Partial**: Basic HTML elements are keyboard accessible
- ❌ No focus indicators styled (browser default only)
- ❌ Tab order not explicitly managed
- ⚠️ Custom buttons (`.action-btn`) should be `<button>` or have proper ARIA

### 5.2 Screen Reader Support

- ⚠️ **Partial**: Using semantic HTML in most places
- ❌ Error messages not associated with form fields
- ❌ No ARIA labels on custom components
- ⚠️ Status badges may need ARIA roles

### 5.3 Color Contrast

- ⚠️ **Needs verification**: Colors defined but contrast not verified
- ⚠️ Hardcoded colors may not meet WCAG AA
- ✅ Design system colors should meet contrast requirements

### 5.4 Form Accessibility

- ❌ **Poor**: Labels use `style="display: block"` instead of proper association
- ❌ Error messages not associated with inputs
- ❌ Required fields not indicated
- ❌ No fieldset/legend for grouped fields

---

## 6. Code Quality & Patterns

### 6.1 Component Structure

- ✅ Consistent ERB partial structure
- ✅ Standard Rails naming conventions
- ⚠️ No TypeScript (not applicable for Rails)

### 6.2 Styling Consistency

- ❌ **Inconsistent**: Mix of Tailwind classes, CSS variables, and hardcoded values
- ❌ Inline styles in views
- ❌ Large custom CSS file (dashboard.css - 1262 lines)
- ⚠️ Tailwind config extends design system but not fully utilized

### 6.3 Reusability

- ⚠️ **Partial**: ERB partials provide some reusability
- ❌ Custom CSS not componentized
- ❌ Form styling not consistent across views
- ✅ Helpers could be created for common patterns

---

## 7. User Experience Patterns

### 7.1 Feedback Mechanisms

- ❌ **Poor**: 
  - No loading states
  - Basic flash messages (inline green/red)
  - No success confirmations
  - Error messages basic (red text)

### 7.2 Interaction Patterns

- ⚠️ **Partial**: 
  - Hover states defined in CSS
  - No active/pressed states visible
  - Disabled states not styled
  - Basic transitions

### 7.3 Responsive Design

- ✅ **Good**: 
  - Media queries in dashboard.css
  - Grid layouts adapt
  - Mobile-friendly breakpoints
  - ⚠️ Touch targets not verified (should be 44x44px)

---

## 8. Dependencies & Integration

### 8.1 Current Dependencies

- ✅ `tailwindcss-rails` gem installed
- ✅ Tailwind config extends design system
- ❌ CSS variables manually duplicated (not imported)

### 8.2 Design System Integration

- ✅ Tailwind config extends design system: `config/tailwind.config.js`
- ❌ CSS variables NOT imported from design system
- ❌ CSS variables manually duplicated in `application.css` and `design-system/css-variables.css`
- ⚠️ Design system Tailwind classes available but not consistently used

---

## 9. Documentation

### 9.1 Component Documentation

- ❌ No component README files
- ❌ No Storybook (not applicable for Rails)
- ❌ No prop/API documentation
- ❌ No usage examples

### 9.2 Style Guide

- ❌ Design tokens not documented in repo
- ❌ Usage guidelines not documented
- ❌ No do's and don'ts

---

## 10. Summary & Priority

### 10.1 Critical Issues

1. **CSS Variables Duplication** - Manually duplicated instead of importing from design system
2. **Form Accessibility** - Labels not properly associated, no error association
3. **Hardcoded Colors** - Many hardcoded hex values in dashboard.css
4. **No Feedback Mechanisms** - Missing loading states, proper error handling
5. **Inconsistent Styling** - Mix of Tailwind, CSS variables, and hardcoded values

### 10.2 Migration Priority

**High Priority** (used everywhere):
- Form components (all CRUD forms)
- Button styling (standardize all buttons)
- Error/feedback messages
- Color system (replace hardcoded values)

**Medium Priority** (used frequently):
- Card components (stat cards, activity cards)
- Typography (standardize font family and sizes)
- Spacing (use design system spacing consistently)

**Low Priority** (used occasionally):
- Dashboard-specific components (may remain custom)
- Chart components (domain-specific)

### 10.3 Estimated Effort

**Quick Wins** (< 1 day each):
- Import CSS variables from design system
- Replace hardcoded colors with CSS variables
- Add proper label associations to forms

**Medium Effort** (1-3 days each):
- Standardize all buttons to use design system classes
- Refactor form styling to use design system patterns
- Add loading/error states

**Large Refactors** (> 3 days each):
- Complete dashboard.css refactor to use design system
- Add modal/alert components (if needed)
- Comprehensive accessibility improvements

---

## Notes & Observations

1. **Tailwind Integration**: Already configured to extend design system, which is good. Need to actually use the classes.

2. **CSS Variables**: Currently duplicated. Should import from `platform/design-system/dist/tokens/css-variables.css` instead.

3. **Forms**: Very basic Rails form helpers. Could benefit from design system form component patterns (even if just CSS classes).

4. **Dashboard**: Large custom CSS file. Could be refactored to use more design system tokens and Tailwind classes.

5. **No JavaScript Components**: Rails app, so React components from design system not directly usable. Need to use CSS/Tailwind approach.

6. **Accessibility**: Needs significant improvement, especially for forms.

---

**Audit Completed By**: System  
**Date**: 2025-01-XX  
**Repository**: pickup-game-manager

