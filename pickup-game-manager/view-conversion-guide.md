# Step-by-Step View Conversion Guide

This guide provides detailed instructions for converting `pickup-game-manager` views from custom styling to design system components.

**Reference Documents:**
- [Component Audit Template](./component-audit-template.md)
- [Refactoring Priority Framework](./refactoring-priority-framework.md)
- [UX Principles Checklist](../design-system/ux-principles-checklist.md)
- [UI Kit Implementation Recommendation](../design-system/ui-kit-implementation-recommendation.md)

---

## Prerequisites

Before starting conversion, ensure:

1. ✅ **Design System CSS Imported**
   - Check `app/assets/stylesheets/application.css` or layout file
   - Should import: `@import "../../platform/design-system/dist/tokens/css-variables.css";`

2. ✅ **Tailwind Config Extended**
   - Check `config/tailwind.config.js`
   - Should extend design system config: `...designSystemConfig.theme.extend`

3. ✅ **Design System Built**
   - Run `cd platform/design-system && npm run build` if needed

4. ✅ **View Audit Completed**
   - Use [Component Audit Template](./component-audit-template.md) to identify what needs updating

---

## Component Class Mappings

### Button Component

**Design System Source**: `platform/design-system/src/components/Button/Button.tsx`

#### Base Classes (Always Required)
```
inline-flex items-center justify-center rounded-lg font-medium transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2 disabled:opacity-50 disabled:pointer-events-none
```

#### Variant Classes

**Primary** (default):
```
bg-primary-600 text-white hover:bg-primary-700 focus:ring-primary-500 active:bg-primary-800 disabled:bg-primary-300
```

**Secondary**:
```
bg-neutral-200 text-neutral-900 hover:bg-neutral-300 focus:ring-neutral-500 active:bg-neutral-400
```

**Text**:
```
text-primary-600 hover:bg-primary-50 focus:ring-primary-500 active:bg-primary-100
```

**Danger**:
```
bg-error-600 text-white hover:bg-error-700 focus:ring-error-500 active:bg-error-800 disabled:bg-error-300
```

**Ghost**:
```
text-neutral-700 hover:bg-neutral-100 focus:ring-neutral-500 active:bg-neutral-200
```

#### Size Classes

**Small** (`sm`):
```
px-3 py-1.5 text-sm gap-1.5
```

**Medium** (`md` - default):
```
px-4 py-2 text-base gap-2
```

**Large** (`lg`):
```
px-6 py-3 text-lg gap-2.5
```

#### Complete Button Examples

**Primary Button (Medium)**:
```erb
<%= link_to "Save", path, class: "inline-flex items-center justify-center rounded-lg font-medium bg-primary-600 text-white hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2" %>
```

**Secondary Button (Medium)**:
```erb
<%= link_to "Cancel", path, class: "inline-flex items-center justify-center rounded-lg font-medium bg-neutral-200 text-neutral-900 hover:bg-neutral-300 focus:outline-none focus:ring-2 focus:ring-neutral-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2" %>
```

**Text Link**:
```erb
<%= link_to "View Details", path, class: "inline-flex items-center justify-center rounded-lg font-medium text-primary-600 hover:bg-primary-50 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2" %>
```

**Danger Button**:
```erb
<%= button_to "Delete", path, method: :delete, class: "inline-flex items-center justify-center rounded-lg font-medium bg-error-600 text-white hover:bg-error-700 focus:outline-none focus:ring-2 focus:ring-error-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2" %>
```

---

### Card Component

**Design System Source**: `platform/design-system/src/components/Card/Card.tsx`

#### Variant Classes

**Default**:
```
rounded-lg bg-white dark:bg-neutral-800
```

**Outlined**:
```
rounded-lg bg-white dark:bg-neutral-800 border border-neutral-200 dark:border-neutral-700
```

**Elevated**:
```
rounded-lg bg-white dark:bg-neutral-800 shadow-md
```

#### Padding Classes

- **None**: (no padding class)
- **Small** (`sm`): `p-4`
- **Medium** (`md` - default): `p-6`
- **Large** (`lg`): `p-8`

#### Card Structure

**Card Header** (optional):
```
mb-4
```

**Card Title**:
```
text-xl font-semibold text-neutral-900 dark:text-neutral-100
```

**Card Content**:
```
text-neutral-700 dark:text-neutral-300
```

**Card Footer** (optional):
```
mt-4 pt-4 border-t border-neutral-200 dark:border-neutral-700
```

#### Complete Card Examples

**Outlined Card with Medium Padding**:
```erb
<div class="rounded-lg bg-white border border-neutral-200 p-6">
  <h3 class="text-xl font-semibold text-neutral-900 mb-4">Card Title</h3>
  <div class="text-neutral-700">
    Card content goes here
  </div>
</div>
```

**Elevated Card**:
```erb
<div class="rounded-lg bg-white shadow-md p-6">
  <!-- Card content -->
</div>
```

---

### Alert Component

**Design System Source**: `platform/design-system/src/components/Alert/Alert.tsx`

#### Base Classes (Always Required)
```
flex gap-3 p-4 rounded-lg border
```

#### Variant Classes

**Success**:
```
bg-success-50 text-success-800 border-success-200
```

**Error**:
```
bg-error-50 text-error-800 border-error-200
```

**Warning**:
```
bg-warning-50 text-warning-800 border-warning-200
```

**Info**:
```
bg-info-50 text-info-800 border-info-200
```

#### Complete Alert Examples

**Success Alert**:
```erb
<div class="flex gap-3 p-4 rounded-lg border bg-success-50 text-success-800 border-success-200" role="alert" aria-live="polite">
  <div class="flex-1 min-w-0">
    <div class="text-sm"><%= notice %></div>
  </div>
</div>
```

**Error Alert with Title**:
```erb
<div class="flex gap-3 p-4 rounded-lg border bg-error-50 text-error-800 border-error-200" role="alert" aria-live="polite">
  <div class="flex-1 min-w-0">
    <h4 class="font-semibold mb-1">Error Title</h4>
    <div class="text-sm">
      <ul class="list-disc list-inside space-y-1">
        <% errors.each do |error| %>
          <li><%= error.full_message %></li>
        <% end %>
      </ul>
    </div>
  </div>
</div>
```

---

### Form Input Component

**Design System Source**: `platform/design-system/src/components/Input/Input.tsx`

#### Label Classes
```
block text-sm font-medium text-neutral-700 dark:text-neutral-300 mb-1
```

**Required Indicator**:
```
text-error-500 ml-1
```

#### Input Base Classes
```
w-full px-4 py-2 text-base text-neutral-900 bg-white border rounded-lg focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent disabled:bg-neutral-100 disabled:text-neutral-500 disabled:cursor-not-allowed placeholder:text-neutral-400
```

#### Input State Classes

**Default**:
```
border-neutral-300 dark:border-neutral-600
```

**Error State**:
```
border-error-500 focus:ring-error-500 focus:border-error-500
```

#### Error Message Classes
```
mt-1 text-sm text-error-600 dark:text-error-400
```

#### Complete Input Example

```erb
<div class="w-full">
  <label for="name" class="block text-sm font-medium text-neutral-700 mb-1">
    Name
    <span class="text-error-500 ml-1">*</span>
  </label>
  <input
    id="name"
    type="text"
    class="w-full px-4 py-2 text-base text-neutral-900 bg-white border border-neutral-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent placeholder:text-neutral-400"
    aria-invalid="false"
  />
</div>
```

**With Error**:
```erb
<div class="w-full">
  <label for="name" class="block text-sm font-medium text-neutral-700 mb-1">
    Name
    <span class="text-error-500 ml-1">*</span>
  </label>
  <input
    id="name"
    type="text"
    class="w-full px-4 py-2 text-base text-neutral-900 bg-white border border-error-500 rounded-lg focus:outline-none focus:ring-2 focus:ring-error-500 focus:border-error-500 placeholder:text-neutral-400"
    aria-invalid="true"
    aria-describedby="name-error"
  />
  <p id="name-error" class="mt-1 text-sm text-error-600" role="alert">
    Name is required
  </p>
</div>
```

---

### Textarea Component

**Design System Source**: `platform/design-system/src/components/Textarea/Textarea.tsx`

#### Textarea Classes
```
w-full px-4 py-2 text-base text-neutral-900 bg-white border rounded-lg focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent disabled:bg-neutral-100 disabled:text-neutral-500 disabled:cursor-not-allowed placeholder:text-neutral-400 resize-y
```

**Same structure as Input** (label, error handling, focus states)

#### Complete Textarea Example

```erb
<div class="w-full">
  <label for="description" class="block text-sm font-medium text-neutral-700 mb-1">
    Description
  </label>
  <textarea
    id="description"
    rows="4"
    class="w-full px-4 py-2 text-base text-neutral-900 bg-white border border-neutral-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent placeholder:text-neutral-400 resize-y"
  ></textarea>
</div>
```

---

### Select Component

**Design System Source**: `platform/design-system/src/components/Select/Select.tsx`

#### Select Classes
```
w-full px-4 py-2 text-base text-neutral-900 bg-white border rounded-lg focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent disabled:bg-neutral-100 disabled:text-neutral-500 disabled:cursor-not-allowed
```

**Same structure as Input** (label, error handling, focus states)

---

### Checkbox Component

**Design System Source**: `platform/design-system/src/components/Checkbox/Checkbox.tsx`

#### Checkbox Container
```
flex items-start gap-3
```

#### Checkbox Input
```
peer h-5 w-5 appearance-none rounded border-2 checked:bg-primary-600 checked:border-primary-600 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 border-neutral-300
```

#### Checkbox Label
```
text-sm font-medium cursor-pointer text-neutral-700
```

---

## Conversion Workflow

### Step 1: Audit Current View

Use the [Component Audit Template](./component-audit-template.md) to:
- Identify all components in the view
- Document current implementation
- Note missing accessibility attributes
- Score compliance

### Step 2: Identify Components to Replace

Create a list of components to update:
- [ ] Buttons/Links
- [ ] Cards
- [ ] Alerts
- [ ] Form inputs
- [ ] Other components

### Step 3: Map Custom Classes to Design System

For each component:
1. Find the current class string
2. Identify the design system equivalent
3. Note any structural changes needed (e.g., adding wrapper divs)

### Step 4: Update HTML Structure

1. **Start with structure** (wrappers, containers)
2. **Update components** one at a time
3. **Add accessibility attributes** as you go
4. **Test after each component** to catch issues early

### Step 5: Add Accessibility Attributes

Ensure:
- Form fields have `aria-describedby` for errors
- Form fields have `aria-invalid` when errors exist
- Alerts have `role="alert"` and `aria-live="polite"`
- Buttons have proper labels
- Focus indicators are visible

### Step 6: Test Responsive Behavior

Test at different breakpoints:
- Mobile (320px - 640px)
- Tablet (641px - 1024px)
- Desktop (1025px+)

### Step 7: Verify UX Principles Compliance

Use the [UX Principles Checklist](../design-system/ux-principles-checklist.md) to verify:
- Clarity & Simplicity
- Visual Hierarchy
- Consistency
- Accessibility
- Mobile Responsiveness

---

## Before/After Examples

### Example 1: Button Conversion

**Before** (Custom):
```erb
<%= link_to "New athlete", new_athlete_path, class: "inline-block rounded-md bg-primary-600 px-4 py-2 text-sm font-medium text-white hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 transition-colors" %>
```

**After** (Design System):
```erb
<%= link_to "New athlete", new_athlete_path, class: "inline-flex items-center justify-center rounded-lg font-medium bg-primary-600 text-white hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2" %>
```

**Key Changes**:
- `inline-block` → `inline-flex items-center justify-center`
- `rounded-md` → `rounded-lg`
- `text-sm` → `text-base` (medium size)
- Added `gap-2` for icon spacing

---

### Example 2: Card Conversion

**Before** (Custom):
```erb
<div id="<%= dom_id athlete %>" class="rounded-lg border border-neutral-200 bg-white p-4 hover:shadow-md transition-shadow">
  <div class="space-y-2">
    <p class="text-sm font-medium text-neutral-700">
      <span class="text-neutral-500">Name:</span>
      <span class="text-neutral-900"><%= athlete.name %></span>
    </p>
  </div>
</div>
```

**After** (Design System):
```erb
<div id="<%= dom_id athlete %>" class="rounded-lg bg-white border border-neutral-200 p-6 hover:shadow-md transition-shadow">
  <div class="text-neutral-700 space-y-2">
    <p class="text-sm font-medium">
      <span class="text-neutral-500">Name:</span>
      <span class="text-neutral-900"><%= athlete.name %></span>
    </p>
  </div>
</div>
```

**Key Changes**:
- `p-4` → `p-6` (medium padding)
- Reordered classes for consistency
- Added `text-neutral-700` to content wrapper (CardContent pattern)

---

### Example 3: Alert Conversion

**Before** (Custom):
```erb
<% if notice.present? %>
  <div class="rounded-lg border border-success-200 bg-success-50 p-4 mb-6" role="alert" aria-live="polite">
    <p class="text-sm text-success-800"><%= notice %></p>
  </div>
<% end %>
```

**After** (Design System):
```erb
<% if notice.present? %>
  <div class="flex gap-3 p-4 rounded-lg border bg-success-50 text-success-800 border-success-200 mb-6" role="alert" aria-live="polite">
    <div class="flex-1 min-w-0">
      <div class="text-sm"><%= notice %></div>
    </div>
  </div>
<% end %>
```

**Key Changes**:
- Added `flex gap-3` for icon support (even if no icon yet)
- Added `flex-1 min-w-0` wrapper for content
- Reordered classes for consistency

---

### Example 4: Form Input Conversion

**Before** (Custom):
```erb
<div class="space-y-2">
  <%= form.label :name, class: "block text-sm font-medium text-neutral-700" %>
  <%= form.text_field :name, 
      class: "w-full rounded-md border border-neutral-300 px-3 py-2 text-sm focus:border-primary-500 focus:outline-none focus:ring-2 focus:ring-primary-500/20",
      aria_describedby: athlete.errors[:name].any? ? "name-error" : nil,
      aria_invalid: athlete.errors[:name].any? %>
  <% if athlete.errors[:name].any? %>
    <p id="name-error" class="text-sm text-error-600" role="alert">
      <%= athlete.errors[:name].first %>
    </p>
  <% end %>
</div>
```

**After** (Design System):
```erb
<div class="w-full">
  <%= form.label :name, class: "block text-sm font-medium text-neutral-700 mb-1" %>
  <%= form.text_field :name, 
      class: "w-full px-4 py-2 text-base text-neutral-900 bg-white border border-neutral-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent placeholder:text-neutral-400",
      aria_describedby: athlete.errors[:name].any? ? "name-error" : nil,
      aria_invalid: athlete.errors[:name].any? %>
  <% if athlete.errors[:name].any? %>
    <p id="name-error" class="mt-1 text-sm text-error-600" role="alert">
      <%= athlete.errors[:name].first %>
    </p>
  <% end %>
</div>
```

**Key Changes**:
- `space-y-2` → `w-full` (full width wrapper)
- Label: Added `mb-1` for spacing
- Input: `rounded-md` → `rounded-lg`
- Input: `px-3 py-2 text-sm` → `px-4 py-2 text-base`
- Input: Added `text-neutral-900 bg-white`
- Input: `focus:ring-primary-500/20` → `focus:ring-primary-500 focus:border-transparent`
- Input: Added `placeholder:text-neutral-400`
- Error: `text-sm` → `mt-1 text-sm` (consistent spacing)

**With Error State**:
```erb
<div class="w-full">
  <%= form.label :name, class: "block text-sm font-medium text-neutral-700 mb-1" %>
  <%= form.text_field :name, 
      class: "w-full px-4 py-2 text-base text-neutral-900 bg-white border border-error-500 rounded-lg focus:outline-none focus:ring-2 focus:ring-error-500 focus:border-error-500 placeholder:text-neutral-400",
      aria_describedby: "name-error",
      aria_invalid: true %>
  <p id="name-error" class="mt-1 text-sm text-error-600" role="alert">
    <%= athlete.errors[:name].first %>
  </p>
</div>
```

---

## Common Patterns

### Page Header Pattern

**Standard Structure**:
```erb
<div class="grid grid-cols-1 md:grid-cols-12 gap-6">
  <div class="md:col-span-12">
    <h1 class="text-3xl font-bold mb-6">Page Title</h1>
  </div>
  <!-- Rest of content -->
</div>
```

### Action Buttons Pattern

**Primary + Secondary Actions**:
```erb
<div class="md:col-span-12 flex gap-4">
  <%= link_to "Primary Action", path, class: "inline-flex items-center justify-center rounded-lg font-medium bg-primary-600 text-white hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2" %>
  <%= link_to "Secondary Action", path, class: "inline-flex items-center justify-center rounded-lg font-medium bg-neutral-200 text-neutral-900 hover:bg-neutral-300 focus:outline-none focus:ring-2 focus:ring-neutral-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2" %>
</div>
```

### Empty State Pattern

```erb
<div class="rounded-lg bg-white border border-neutral-200 p-6 text-center">
  <p class="text-neutral-500 mb-4">No items found</p>
  <%= link_to "Create First Item", new_path, class: "inline-flex items-center justify-center rounded-lg font-medium bg-primary-600 text-white hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2" %>
</div>
```

### Form Layout Pattern

```erb
<%= form_with(model: resource, class: "space-y-6") do |form| %>
  <% if resource.errors.any? %>
    <!-- Error alert -->
  <% end %>

  <div class="w-full">
    <!-- Form fields -->
  </div>

  <div class="flex gap-4">
    <%= form.submit class: "inline-flex items-center justify-center rounded-lg font-medium bg-primary-600 text-white hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2" %>
    <%= link_to "Cancel", cancel_path, class: "inline-flex items-center justify-center rounded-lg font-medium bg-neutral-200 text-neutral-900 hover:bg-neutral-300 focus:outline-none focus:ring-2 focus:ring-neutral-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2" %>
  </div>
<% end %>
```

---

## Troubleshooting

### Issue: Classes Not Applying

**Symptoms**: Design system classes don't work, styles not showing

**Solutions**:
1. Verify Tailwind config extends design system: `config/tailwind.config.js`
2. Check CSS import in layout: `app/views/layouts/application.html.erb`
3. Rebuild Tailwind: `rails tailwindcss:build`
4. Clear cache: `rails tmp:clear`

### Issue: Colors Not Matching

**Symptoms**: Colors look different from design system

**Solutions**:
1. Verify CSS variables imported: Check `application.css` for `@import "../../platform/design-system/dist/tokens/css-variables.css"`
2. Rebuild design system: `cd platform/design-system && npm run build`
3. Check Tailwind config includes design system colors

### Issue: Focus States Not Visible

**Symptoms**: Can't see focus rings on keyboard navigation

**Solutions**:
1. Ensure `focus:outline-none focus:ring-2` classes are present
2. Check `focus:ring-offset-2` for proper spacing
3. Test with keyboard navigation (Tab key)

### Issue: Form Validation Not Styling Correctly

**Symptoms**: Error states not showing proper styling

**Solutions**:
1. Verify error state classes: `border-error-500 focus:ring-error-500`
2. Check `aria-invalid` attribute is set correctly
3. Ensure error message has `id` matching `aria-describedby`

### Issue: Responsive Layout Breaking

**Symptoms**: Layout breaks on mobile/tablet

**Solutions**:
1. Verify grid classes: `grid grid-cols-1 md:grid-cols-12`
2. Check spacing classes work at all breakpoints
3. Test with browser dev tools responsive mode

### Issue: Button Icons Not Aligning

**Symptoms**: Icons misaligned in buttons

**Solutions**:
1. Ensure `inline-flex items-center justify-center` classes
2. Add `gap-2` (or appropriate gap) between icon and text
3. Verify icon has proper size classes

---

## Quick Reference: Class Cheat Sheet

### Button (Primary, Medium)
```
inline-flex items-center justify-center rounded-lg font-medium bg-primary-600 text-white hover:bg-primary-700 focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2 transition-colors px-4 py-2 text-base gap-2
```

### Card (Outlined, Medium Padding)
```
rounded-lg bg-white border border-neutral-200 p-6
```

### Alert (Success)
```
flex gap-3 p-4 rounded-lg border bg-success-50 text-success-800 border-success-200
```

### Input (Default)
```
w-full px-4 py-2 text-base text-neutral-900 bg-white border border-neutral-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-transparent placeholder:text-neutral-400
```

### Input (Error)
```
w-full px-4 py-2 text-base text-neutral-900 bg-white border border-error-500 rounded-lg focus:outline-none focus:ring-2 focus:ring-error-500 focus:border-error-500 placeholder:text-neutral-400
```

---

## Next Steps

After converting a view:

1. ✅ Run the [Component Audit Template](./component-audit-template.md) again to verify compliance
2. ✅ Test all user flows
3. ✅ Verify accessibility with keyboard navigation
4. ✅ Test responsive behavior
5. ✅ Check UX principles compliance
6. ✅ Document any exceptions or special cases

---

**Guide Version**: 1.0

**Last Updated**: _________________

**Questions or Issues?** Refer to the troubleshooting section or check the design system component source files.

