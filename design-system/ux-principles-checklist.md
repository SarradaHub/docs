# UX Principles Checklist

Use this checklist for every new component or page ticket to ensure consistent user experience and adherence to design system best practices.

**Reference Sources:**
- [UI Design Best Practices 2025](https://www.webstacks.com/blog/ui-design-best-practices)
- [UX Design in 2026: A Realistic Roadmap](https://andreasyanaram.medium.com/ux-design-in-2026-a-realistic-roadmap-4d3aca064a8b)
- [UX Designer Roadmap 2026](https://uxplanet.org/ux-designer-roadmap-2026-ai-vibe-coding-9d3da2f8d690)

---

## 1. Clarity & Simplicity

### Visual Clarity
- [ ] **Purpose is immediately clear** - Users understand what the component/page does without explanation
- [ ] **Unnecessary elements removed** - Every button, image, and text block serves a clear purpose
- [ ] **Single primary action** - One main call-to-action per view (not multiple competing CTAs)
- [ ] **Adequate white space** - Key elements have breathing room and stand out
- [ ] **Logical grouping** - Related information is grouped together visually

### Content Clarity
- [ ] **Clear, action-oriented labels** - Button labels are specific (e.g., "Save Changes" not "Submit")
- [ ] **Concise copy** - Text is brief and scannable
- [ ] **No jargon** - Language is accessible to all users
- [ ] **Progressive disclosure** - Complex information is revealed progressively, not all at once

---

## 2. Visual Hierarchy

### Information Priority
- [ ] **Most important information is most prominent** - Size, color, and position emphasize key content
- [ ] **Primary actions stand out** - Main CTA is more prominent than secondary options
- [ ] **Headings create clear structure** - H1 > H2 > H3 hierarchy is logical and consistent
- [ ] **Visual weight matches importance** - Important elements have more visual weight

### Design Elements
- [ ] **Size hierarchy** - Larger elements draw attention first
- [ ] **Color contrast** - Important elements use higher contrast
- [ ] **Spacing creates separation** - Related items are close, unrelated items are separated
- [ ] **Typography hierarchy** - Font sizes and weights create clear information levels

---

## 3. Consistency

### Design Patterns
- [ ] **Uses design system components** - No custom components when design system has equivalent
- [ ] **Consistent button styles** - Same button style for similar actions throughout
- [ ] **Consistent spacing** - Same spacing scale used (8px base unit)
- [ ] **Consistent colors** - Design system color tokens used, no hardcoded colors
- [ ] **Consistent typography** - Design system typography scale used

### Interaction Patterns
- [ ] **Uniform navigation** - Navigation patterns consistent across pages
- [ ] **Same interaction patterns** - Hover states, click behaviors match site-wide patterns
- [ ] **Consistent icon usage** - Same icon for same concept (e.g., gear icon = settings everywhere)
- [ ] **Consistent form patterns** - Forms follow same structure and validation approach

### Component API
- [ ] **Props follow design system conventions** - Component APIs match design system patterns
- [ ] **Naming is consistent** - Prop names match design system (e.g., `variant`, `size`, not `type`, `scale`)

---

## 4. User Journey & Flow

### Task Completion
- [ ] **Intuitive user path** - Users can complete tasks without confusion
- [ ] **Reduced cognitive load** - Information is presented in digestible chunks
- [ ] **Clear next steps** - Users know what to do next at each stage
- [ ] **Minimal steps to goal** - User journey is optimized (fewest clicks/actions possible)

### Navigation & Orientation
- [ ] **Clear current location** - Users know where they are (breadcrumbs, active nav state)
- [ ] **Easy navigation** - Users can easily move between related content
- [ ] **Logical flow** - Page sequence makes sense (e.g., list → detail → edit)
- [ ] **Escape hatches** - Users can easily go back or cancel actions

### Form Optimization
- [ ] **Only necessary fields** - No optional fields unless truly needed (reduce by 20-60% if possible)
- [ ] **Logical field order** - Fields flow in natural order
- [ ] **Inline validation** - Errors shown immediately, not on submit
- [ ] **Auto-fill enabled** - Browser autofill works where appropriate
- [ ] **Multi-step forms** - Long forms broken into logical steps

---

## 5. Feedback & Response

### Action Feedback
- [ ] **Immediate visual feedback** - All interactions provide instant feedback (hover, click, focus)
- [ ] **Loading states** - Actions taking >500ms show loading indicators
- [ ] **Success confirmations** - Significant actions show success messages
- [ ] **Clear error messages** - Errors are specific and actionable (not just "Error occurred")
- [ ] **Micro-animations** - Subtle animations acknowledge user interactions

### State Communication
- [ ] **Button states visible** - Default, hover, active, disabled, loading states all clear
- [ ] **Form validation feedback** - Real-time validation with clear error messages
- [ ] **Empty states** - Empty lists/tables have helpful messages
- [ ] **Loading states** - Skeleton screens or spinners during data fetch

### Error Handling
- [ ] **User-friendly error messages** - Technical errors translated to user language
- [ ] **Actionable solutions** - Errors include suggestions for resolution
- [ ] **Graceful degradation** - Component handles errors without breaking entire page

---

## 6. Accessibility (a11y)

### Keyboard Navigation
- [ ] **Fully keyboard navigable** - All interactive elements accessible via keyboard
- [ ] **Logical tab order** - Tab sequence follows visual order
- [ ] **Focus indicators visible** - Focus states are clearly visible (not just outline: none)
- [ ] **Keyboard shortcuts** - Common actions have keyboard shortcuts where appropriate
- [ ] **Focus management** - Focus moves appropriately (modals, dynamic content)

### Screen Reader Support
- [ ] **Semantic HTML** - Correct HTML elements used (button, not div with onClick)
- [ ] **ARIA labels** - Interactive elements have appropriate ARIA labels
- [ ] **ARIA roles** - Complex components have proper ARIA roles
- [ ] **Alt text for images** - All images have descriptive alt text
- [ ] **Form labels** - All form inputs have associated labels
- [ ] **Error announcements** - Screen readers announce errors appropriately

### Visual Accessibility
- [ ] **Color contrast** - Text meets WCAG AA (4.5:1 for normal, 3:1 for large)
- [ ] **Not color-dependent** - Information not conveyed by color alone
- [ ] **Text alternatives** - Icons have text labels or aria-labels
- [ ] **Resizable text** - Text can be resized up to 200% without breaking layout

### Form Accessibility
- [ ] **Label association** - Labels properly associated with inputs (for/id attributes)
- [ ] **Required field indication** - Required fields clearly marked (not just asterisk)
- [ ] **Error association** - Error messages associated with form fields
- [ ] **Fieldset/legend** - Related fields grouped with fieldset/legend

---

## 7. Mobile & Responsive Design

### Mobile Optimization
- [ ] **Mobile-first approach** - Designed for mobile, enhanced for desktop
- [ ] **Touch targets adequate** - Interactive elements at least 44x44 pixels
- [ ] **Thumb-friendly navigation** - Navigation accessible with thumb (bottom navigation, etc.)
- [ ] **Content prioritization** - Most important content visible on mobile without scrolling
- [ ] **Fast load times** - Optimized for mobile networks

### Responsive Behavior
- [ ] **Breakpoints used consistently** - Same breakpoints as design system
- [ ] **Layout adapts gracefully** - Layout works at all screen sizes
- [ ] **Text readable** - Text size appropriate for screen size
- [ ] **Images responsive** - Images scale appropriately
- [ ] **Navigation adapts** - Mobile menu/hamburger for small screens

### Performance
- [ ] **Optimized images** - Images are appropriately sized and optimized
- [ ] **Lazy loading** - Below-fold content loads lazily
- [ ] **Minimal JavaScript** - Only necessary JavaScript loaded

---

## 8. Layout & Structure

### Grid System
- [ ] **Consistent layout grid** - Uses design system grid (12-column or specified)
- [ ] **Visual alignment** - Elements align to grid
- [ ] **Consistent gutters** - Spacing between columns is consistent
- [ ] **Max-width container** - Content doesn't stretch too wide on large screens

### Component Structure
- [ ] **Standardized component structure** - Follows design system component patterns
- [ ] **Composition over duplication** - Uses composition to build complex components
- [ ] **Consistent prop APIs** - Props follow design system conventions
- [ ] **CSS methodology consistent** - Uses same approach (Tailwind/BEM/etc.) as design system

---

## 9. Performance & Optimization

### Loading Performance
- [ ] **Fast initial load** - Page loads quickly (< 3 seconds on 3G)
- [ ] **Progressive enhancement** - Core functionality works without JavaScript
- [ ] **Code splitting** - Large components lazy-loaded
- [ ] **Optimized assets** - Images, fonts, and other assets optimized

### Runtime Performance
- [ ] **Smooth interactions** - Animations run at 60fps
- [ ] **No layout shifts** - Content doesn't shift during load (CLS)
- [ ] **Efficient re-renders** - React components optimized (memo, useMemo where needed)

---

## 10. Testing & Validation

### User Testing
- [ ] **Usability tested** - Component/page tested with real users (if applicable)
- [ ] **Edge cases handled** - Empty states, error states, loading states all handled
- [ ] **Cross-browser tested** - Works in major browsers (Chrome, Firefox, Safari, Edge)

### Technical Validation
- [ ] **Accessibility tested** - Tested with screen reader and keyboard navigation
- [ ] **Responsive tested** - Tested at multiple screen sizes
- [ ] **Performance validated** - Meets performance benchmarks
- [ ] **Design system compliance** - Uses design system components and tokens

---

## Quick Reference: Critical Items

**Must Have (Blockers):**
- ✅ Uses design system components
- ✅ Keyboard navigable
- ✅ Color contrast meets WCAG AA
- ✅ Clear primary action
- ✅ Loading/error states handled
- ✅ Mobile responsive

**Should Have (Important):**
- ✅ Consistent with design patterns
- ✅ Clear visual hierarchy
- ✅ Immediate feedback for interactions
- ✅ Accessible to screen readers
- ✅ Optimized for performance

**Nice to Have (Enhancements):**
- ✅ Micro-animations
- ✅ Advanced keyboard shortcuts
- ✅ Skeleton loading states
- ✅ Advanced accessibility features

---

## Notes

_Use this space to document any exceptions, special considerations, or additional context for this component/page:_

_______________________________________________________________________
_______________________________________________________________________
_______________________________________________________________________

---

**Component/Page**: _______________________
**Ticket/Issue**: _______________________
**Reviewed By**: _______________________
**Date**: _______________________

