# Refactoring Priority Framework

This framework provides a systematic approach to determine which views in `pickup-game-manager` should be updated first to use the design system components.

**Reference Documents:**
- [Component Audit Template](./component-audit-template.md)
- [View Conversion Guide](./view-conversion-guide.md)
- [UX Principles Checklist](../design-system/ux-principles-checklist.md)

---

## Priority Scoring Matrix

Each view is scored on five criteria (1-5 scale, where 5 is highest priority). The total score determines the priority tier.

### Scoring Criteria

#### 1. User Impact (1-5)
- **5**: Critical user flows, high-traffic pages, entry points
- **4**: Frequently accessed pages, important workflows
- **3**: Regularly used pages
- **2**: Occasionally accessed pages
- **1**: Rarely used pages, admin/internal tools

**Examples:**
- Dashboard: 5 (main entry point)
- Index pages (athletes, matches): 4 (frequently accessed)
- Show pages: 3 (regularly used)
- Edit pages: 2 (less frequent)

#### 2. Technical Debt (1-5)
- **5**: Heavy custom CSS, many inline styles, complex custom components
- **4**: Significant custom styling, multiple custom classes
- **3**: Some custom styling, mixed patterns
- **2**: Minimal custom styling, mostly standard
- **1**: Already using design system patterns

**Examples:**
- Dashboard: 5 (custom CSS classes, complex layout)
- Forms: 4 (custom input styling, validation display)
- Index pages: 3 (custom card styling, button classes)
- Partials: 2-3 (depends on implementation)

#### 3. Dependencies (1-5)
- **5**: Foundation views that other views depend on (partials, layouts)
- **4**: Views that are reused across multiple pages
- **3**: Views that are referenced by a few other views
- **2**: Standalone views with minimal dependencies
- **1**: Completely isolated views

**Examples:**
- Partials (`_athlete.html.erb`, `_form.html.erb`): 5 (used in multiple views)
- Layout (`application.html.erb`): 5 (affects all pages)
- Index pages: 3 (may use partials)
- Show pages: 2 (standalone, may use partials)

#### 4. Consistency Deviation (1-5)
- **5**: Completely different from design system, major inconsistencies
- **4**: Significant deviations, noticeable inconsistencies
- **3**: Some deviations, minor inconsistencies
- **2**: Mostly consistent with minor differences
- **1**: Already consistent with design system

**Examples:**
- Dashboard: 5 (custom stat cards, action buttons, charts)
- Forms: 4 (custom input styling, error display)
- Index pages: 3 (custom card styling, button classes)
- Show pages: 2 (mostly consistent, minor button differences)

#### 5. Maintenance Frequency (1-5)
- **5**: Frequently updated, active development
- **4**: Regularly updated
- **3**: Occasionally updated
- **2**: Rarely updated
- **1**: Stable, no changes expected

**Examples:**
- Forms: 5 (often need field additions/changes)
- Dashboard: 4 (may need new sections)
- Index pages: 3 (occasional layout changes)
- Show pages: 2 (rarely changed)

### Priority Tier Calculation

**Total Score = User Impact + Technical Debt + Dependencies + Consistency Deviation + Maintenance Frequency**

- **P0 (Critical)**: 20-25 points
- **P1 (High)**: 15-19 points
- **P2 (Medium)**: 10-14 points
- **P3 (Low)**: 5-9 points

---

## View Inventory & Priority Assignments

### P0 (Critical) - Update First

#### Dashboard (`dashboard/index.html.erb`)
- **User Impact**: 5 (main entry point, high traffic)
- **Technical Debt**: 5 (heavy custom CSS, complex components)
- **Dependencies**: 3 (standalone but foundational)
- **Consistency Deviation**: 5 (custom stat cards, action buttons, charts)
- **Maintenance Frequency**: 4 (may need new sections)
- **Total Score**: 22
- **Rationale**: Main entry point, complex UI with many custom components, sets the tone for the app

#### Layout (`layouts/application.html.erb`)
- **User Impact**: 5 (affects all pages)
- **Technical Debt**: 2 (minimal, mostly structure)
- **Dependencies**: 5 (all views depend on this)
- **Consistency Deviation**: 2 (mostly consistent)
- **Maintenance Frequency**: 1 (rarely changed)
- **Total Score**: 15
- **Rationale**: Foundation for all views, ensures design system CSS is properly loaded

---

### P1 (High) - Update Soon

#### Form Partials (`_form.html.erb` files)
- **User Impact**: 4 (forms are critical user flows)
- **Technical Debt**: 4 (custom input styling, validation)
- **Dependencies**: 5 (used in new/edit views)
- **Consistency Deviation**: 4 (custom input styling, error display)
- **Maintenance Frequency**: 5 (frequently updated)
- **Total Score**: 22
- **Rationale**: Reusable across multiple views, affects all form pages, high maintenance

**Affected Files:**
- `athletes/_form.html.erb`
- `matches/_form.html.erb`
- `expenses/_form.html.erb`
- `payments/_form.html.erb`
- `incomes/_form.html.erb`

#### Index Pages (List Views)
- **User Impact**: 4 (frequently accessed)
- **Technical Debt**: 3 (custom card styling, button classes)
- **Dependencies**: 2 (standalone, may use partials)
- **Consistency Deviation**: 3 (custom card styling, button classes)
- **Maintenance Frequency**: 3 (occasionally updated)
- **Total Score**: 15
- **Rationale**: High visibility, establishes patterns for list views

**Affected Files:**
- `athletes/index.html.erb` (Score: 15)
- `matches/index.html.erb` (Score: 15)
- `expenses/index.html.erb` (Score: 15)
- `payments/index.html.erb` (Score: 15)
- `incomes/index.html.erb` (Score: 15)

#### Show Pages (Detail Views)
- **User Impact**: 3 (regularly used)
- **Technical Debt**: 2 (mostly standard, some custom buttons)
- **Dependencies**: 2 (standalone, may use partials)
- **Consistency Deviation**: 2 (mostly consistent, minor button differences)
- **Maintenance Frequency**: 2 (rarely updated)
- **Total Score**: 11
- **Rationale**: Important for user experience, relatively simple to update

**Affected Files:**
- `athletes/show.html.erb` (Score: 11)
- `matches/show.html.erb` (Score: 11)
- `expenses/show.html.erb` (Score: 11)
- `payments/show.html.erb` (Score: 11)
- `incomes/show.html.erb` (Score: 11)

#### New/Edit Pages
- **User Impact**: 4 (critical user flows)
- **Technical Debt**: 3 (uses form partials)
- **Dependencies**: 3 (uses form partials)
- **Consistency Deviation**: 3 (depends on form partials)
- **Maintenance Frequency**: 4 (regularly updated)
- **Total Score**: 17
- **Rationale**: Critical flows, but depends on form partials (update after forms)

**Affected Files:**
- `athletes/new.html.erb` (Score: 17)
- `athletes/edit.html.erb` (Score: 17)
- `matches/new.html.erb` (Score: 17)
- `matches/edit.html.erb` (Score: 17)
- `expenses/new.html.erb` (Score: 17)
- `expenses/edit.html.erb` (Score: 17)
- `payments/new.html.erb` (Score: 17)
- `payments/edit.html.erb` (Score: 17)
- `incomes/new.html.erb` (Score: 17)
- `incomes/edit.html.erb` (Score: 17)

#### Entity Partials (Card Components)
- **User Impact**: 3 (used in list views)
- **Technical Debt**: 3 (custom card styling)
- **Dependencies**: 4 (used in index pages)
- **Consistency Deviation**: 3 (custom card styling)
- **Maintenance Frequency**: 2 (rarely updated)
- **Total Score**: 15
- **Rationale**: Reusable components, affects multiple index pages

**Affected Files:**
- `athletes/_athlete.html.erb` (Score: 15)
- `matches/_match.html.erb` (Score: 15)
- `expenses/_expense.html.erb` (Score: 15)
- `payments/_payment.html.erb` (Score: 15)
- `incomes/_income.html.erb` (Score: 15)

---

### P2 (Medium) - Update Later

#### Mailer Layout (`layouts/mailer.html.erb`)
- **User Impact**: 2 (email templates, less visible)
- **Technical Debt**: 2 (minimal styling)
- **Dependencies**: 1 (isolated)
- **Consistency Deviation**: 2 (mostly consistent)
- **Maintenance Frequency**: 1 (rarely updated)
- **Total Score**: 8
- **Rationale**: Low priority, email templates have different constraints

---

### P3 (Low) - Nice to Have

#### JSON Views (`*.json.jbuilder`)
- **User Impact**: 1 (API responses, not UI)
- **Technical Debt**: 1 (no UI components)
- **Dependencies**: 1 (isolated)
- **Consistency Deviation**: 1 (not applicable)
- **Maintenance Frequency**: 2 (rarely updated)
- **Total Score**: 6
- **Rationale**: Not UI-related, no design system components needed

---

## Dependency Graph

### Foundation Layer (Update First)
```
layouts/application.html.erb
  └── All views depend on this
```

### Core Components Layer (Update Second)
```
_form.html.erb (all forms)
  └── Used by: new.html.erb, edit.html.erb

_athlete.html.erb, _match.html.erb, etc.
  └── Used by: index.html.erb, show.html.erb
```

### Page Views Layer (Update Third)
```
dashboard/index.html.erb
  └── Standalone, but foundational

index.html.erb (all resources)
  └── Uses: _athlete.html.erb, _match.html.erb, etc.

show.html.erb (all resources)
  └── Uses: _athlete.html.erb, _match.html.erb, etc.

new.html.erb, edit.html.erb (all resources)
  └── Uses: _form.html.erb
```

---

## Recommended Update Order

### Phase 1: Foundation (Week 1)
1. ✅ **Layout** (`layouts/application.html.erb`)
   - Ensure design system CSS is loaded
   - Verify Tailwind config is extended
   - Minimal changes needed

### Phase 2: Core Components (Week 1-2)
2. **Form Partials** (all `_form.html.erb` files)
   - Standardize input styling
   - Update error display
   - Add accessibility attributes
   - **Impact**: Affects all new/edit pages

3. **Entity Partials** (all `_*.html.erb` card components)
   - Convert to design system Card
   - Standardize structure
   - **Impact**: Affects all index/show pages

### Phase 3: High-Traffic Pages (Week 2-3)
4. **Dashboard** (`dashboard/index.html.erb`)
   - Convert stat cards to design system Card
   - Update action buttons
   - Standardize alert messages
   - **Impact**: Main entry point, high visibility

5. **Index Pages** (all `index.html.erb` files)
   - Update to use standardized partials
   - Standardize buttons
   - Update alerts
   - **Impact**: Frequently accessed pages

### Phase 4: Supporting Pages (Week 3-4)
6. **Show Pages** (all `show.html.erb` files)
   - Update buttons to design system
   - Standardize layout
   - **Impact**: Important for UX, relatively simple

7. **New/Edit Pages** (all `new.html.erb`, `edit.html.erb` files)
   - Should be mostly done after form partials
   - Verify consistency
   - **Impact**: Critical flows, but depends on Phase 2

---

## Risk Assessment

### Breaking Changes

#### Low Risk
- **Layout changes**: Minimal risk, mostly structural
- **Button styling**: Low risk, visual changes only
- **Card styling**: Low risk, visual changes only

#### Medium Risk
- **Form changes**: Medium risk, could affect validation display
  - **Mitigation**: Test all forms thoroughly, ensure error messages still work
- **Alert changes**: Medium risk, could affect user feedback
  - **Mitigation**: Verify all alert types work correctly

#### High Risk
- **Dashboard changes**: High risk, complex layout
  - **Mitigation**: Test thoroughly, consider staging environment
  - **Rollback plan**: Keep backup of original files

### User Disruption

#### Minimal Disruption
- Visual changes only (colors, spacing)
- Improved consistency actually reduces confusion

#### Potential Disruption
- Form validation changes (if not tested properly)
- Button placement changes (if layout changes)

**Mitigation Strategy:**
1. Test all user flows after updates
2. Use feature flags if possible
3. Deploy to staging first
4. Monitor user feedback

---

## Success Metrics

### Before Refactoring
- [ ] Document current component usage
- [ ] Measure current accessibility scores
- [ ] Capture current design inconsistencies

### After Refactoring
- [ ] All views use design system components
- [ ] Accessibility scores improved
- [ ] Design consistency achieved
- [ ] Reduced custom CSS
- [ ] Faster development (reusable components)

### Tracking
- **Component Usage**: Track which design system components are used
- **Custom CSS**: Measure reduction in custom CSS
- **Accessibility**: Track accessibility improvements
- **Development Speed**: Measure time to create new views

---

## Notes

_Use this space to document any exceptions, special considerations, or changes to priorities:_

_______________________________________________________________________
_______________________________________________________________________
_______________________________________________________________________

---

**Framework Version**: 1.0

**Last Updated**: _________________

**Next Review**: _________________

