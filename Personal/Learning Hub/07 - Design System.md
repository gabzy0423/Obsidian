---
title: "07 — Design System"
project: "The Learning Lab Website Redesign and Redevelopment"
document_type: "Visual Design System and Component Standards"
status: "Proposed — For Review"
primary_reference: "https://thefoundrylab.com/"
secondary_reference: "Provided HTML Concept Prototype"
scope: "Public-Facing Corporate Website Prototype"
---

# 07 — Design System

## The Learning Lab — Website Redesign & Redevelopment

> [!abstract] Document Information
> **Document Type:** Visual Design System & Component Standards
> **Project Status:** Proposed — For Review
> **Primary Reference:** [thefoundrylab.com](https://thefoundrylab.com/)
> **Secondary Reference:** Provided HTML Concept Prototype
> **Project Scope:** Public-Facing Corporate Website Prototype

---

## 1. Document Overview

This document establishes the proposed design system for The Learning Lab's website redevelopment.

The design system defines the visual foundations, interface components, layout conventions, and styling rules that will guide the creation of a cohesive corporate website.

It aims to ensure that all website pages share a consistent visual identity while supporting usability, readability, accessibility, and responsive behavior.

The provided HTML prototype serves as an initial reference for the proposed visual direction. However, its styling decisions remain subject to review against the organization's official branding requirements.

The design system will serve as the primary visual reference for frontend implementation and interface quality assurance.

### 1.1 Related Documents

- [[01 - Project Overview]]
- [[02 - Existing Website Audit]]
- [[03 - Website Redevelopment Strategy]]
- [[04 - Information Architecture]]
- [[05 - Content Strategy]]
- [[06 - Page-by-Page Blueprint]]

### 1.2 Design System Status

The specifications in this document represent a proposed design direction.

Final brand colors, typography, logo usage, and visual assets must be approved before production implementation.

---

## 2. Design System Objectives

The design system aims to:

1. Establish a consistent visual identity across all website pages.
2. Define reusable design tokens for colors, typography, spacing, and layout.
3. Standardize common interface components.
4. Improve readability and visual hierarchy.
5. Support responsive layouts across desktop, tablet, and mobile devices.
6. Maintain accessible contrast and interaction states.
7. Reduce inconsistent styling and duplicated frontend code.
8. Simplify future design refinements.
9. Support efficient implementation of the website prototype.
10. Align the final interface with approved organizational branding.

---

## 3. Visual Design Direction

The proposed website should follow a modern corporate design approach that balances professionalism with an engaging learning-focused identity.

### 3.1 Design Characteristics

**Professional**

The interface should communicate credibility through clear layouts, readable typography, and consistent styling.

**Approachable**

The design should feel welcoming rather than overly formal or technical.

**Structured**

Content should follow a clear visual hierarchy with intentional spacing.

**Engaging**

Visual accents and interactive elements should support the organization's learning-oriented identity.

**Minimal**

Unnecessary decoration, excessive animation, and overly complicated layouts should be avoided.

**Consistent**

Similar components should follow the same styling and interaction conventions.

### 3.2 Design Principles

- Prioritize content readability.
- Use whitespace intentionally.
- Maintain consistent visual hierarchy.
- Avoid excessive decorative elements.
- Use accent colors purposefully.
- Prefer reusable components.
- Keep animations subtle and functional.
- Ensure layouts remain accessible and responsive.

---

## 4. Proposed Color System

The HTML prototype provides an initial color direction using purple, mint, orange, coral, and off-white.

These colors will serve as provisional design tokens until the organization confirms its official palette.

### 4.1 Primary Color Palette

| Color | Hex Code | Proposed Usage |
|---|---|---|
| Deep Purple | `#281052` | Primary brand surfaces, headings, dark sections |
| Mint | `#55DFBE` | Accent elements and selected interactive highlights |
| Orange | `#FFBD59` | Supporting highlights and visual accents |
| Coral | `#FF6875` | Limited emphasis and decorative elements |
| Off-White | `#F8F6FC` | Soft section backgrounds |

> [!caution]
> This is a proposed visual palette based on the previously documented HTML prototype. It is not yet approved as official branding.

### 4.2 Neutral Colors

A neutral palette is required to support text, borders, backgrounds, and interface states.

| Token | Proposed Value | Usage |
|---|---|---|
| White | `#FFFFFF` | Main backgrounds and cards |
| Near Black | `#171321` | Primary body text |
| Dark Gray | `#514B5D` | Secondary text |
| Light Gray | `#E6E2EB` | Borders and dividers |
| Soft Gray | `#F2F0F5` | Muted backgrounds |

These neutral values are proposed additions to the prototype palette.

### 4.3 Semantic Color Tokens

Semantic tokens should describe the purpose of a color rather than its appearance.

| Token | Purpose |
|---|---|
| `color-background` | Main page background |
| `color-surface` | Cards and panels |
| `color-surface-muted` | Alternate section background |
| `color-text-primary` | Main readable text |
| `color-text-secondary` | Supporting text |
| `color-brand-primary` | Primary brand color |
| `color-accent` | Accent color |
| `color-border` | Borders and dividers |
| `color-focus` | Keyboard focus indicators |
| `color-error` | Form errors |
| `color-success` | Validated success states |

### 4.4 Color Usage Rules

1. Deep Purple should establish the primary visual identity.
2. White and Off-White should provide comfortable reading surfaces.
3. Mint, Orange, and Coral should be used selectively.
4. Avoid placing low-contrast text on colored backgrounds.
5. Do not rely on color alone to communicate meaning.
6. Use consistent semantic colors across components.
7. Validate text and control contrast before final approval.

### 4.5 Color Accessibility

The design should target WCAG 2.2 AA contrast requirements:

- Normal text: at least 4.5:1.
- Large text: at least 3:1.
- Essential non-text UI indicators: at least 3:1 against adjacent colors where applicable.

Accent colors should not automatically be used as text colors.

For example, Mint on White may be visually appealing but may not provide sufficient contrast for ordinary body text.

---

## 5. Typography System

The HTML prototype uses Outfit and DM Sans as its proposed font pairing.

These fonts may be retained if they align with the organization's approved brand identity.

### 5.1 Proposed Font Families

| Font | Usage |
|---|---|
| Outfit | Headings, titles, display text |
| DM Sans | Body text, labels, navigation, forms |
| System Sans-Serif | Fallback |

### 5.2 Typography Hierarchy

| Style | Desktop Size | Mobile Size | Weight |
|---|---:|---:|---:|
| Display | 56px | 36px | 700 |
| H1 | 44px | 32px | 700 |
| H2 | 36px | 28px | 700 |
| H3 | 28px | 24px | 600 |
| H4 | 22px | 20px | 600 |
| Body Large | 18px | 17px | 400 |
| Body | 16px | 16px | 400 |
| Body Small | 14px | 14px | 400 |
| Caption | 12px | 12px | 500 |

These values are initial design targets and may be adjusted after testing.

### 5.3 Line Height

| Text Type | Recommended Line Height |
|---|---|
| Display / Headings | 1.1–1.3 |
| Body Text | 1.5–1.7 |
| Labels | 1.3–1.5 |

### 5.4 Typography Rules

- Use one primary H1 per page.
- Maintain a logical heading hierarchy.
- Avoid using too many font weights.
- Keep body text readable.
- Use consistent paragraph spacing.
- Avoid long lines of text.
- Use responsive font sizing for large headings.
- Do not use decorative fonts for essential content.

---
# Part VI — Spacing System

## 6. Spacing Scale

The website uses a consistent spacing scale based on a 4px unit.

| Token | Value | Typical usage |
|---|---:|---|
| `space-1` | 4px | Small icon gaps |
| `space-2` | 8px | Tight element spacing |
| `space-3` | 12px | Compact component spacing |
| `space-4` | 16px | Standard spacing |
| `space-5` | 20px | Form and card spacing |
| `space-6` | 24px | Component padding |
| `space-8` | 32px | Content groups |
| `space-10` | 40px | Large content groups |
| `space-12` | 48px | Section subdivisions |
| `space-16` | 64px | Major section spacing |
| `space-20` | 80px | Large desktop sections |
| `space-24` | 96px | Hero and major sections |

### 6.1 Section Spacing

| Device | Proposed vertical section padding |
|---|---:|
| Desktop | 80–96px |
| Tablet | 64–80px |
| Mobile | 48–64px |

### 6.2 Spacing Rules

- Use spacing tokens instead of arbitrary values.
- Keep consistent gaps between related elements.
- Increase spacing between unrelated content groups.
- Avoid excessive empty space on mobile.
- Ensure cards have consistent internal padding.
- Use whitespace to support hierarchy rather than decoration.

---

# Part VII — Layout and Grid System

## 7. Responsive Content Layout

The website uses a responsive content container with consistent horizontal alignment.

### 7.1 Container Widths

| Container | Proposed maximum width |
|---|---:|
| Main content | 1200px |
| Wide content | 1320px |
| Reading content | 760px |

### 7.2 Page Margins

| Device | Horizontal padding |
|---|---:|
| Desktop | 40px |
| Tablet | 24px |
| Mobile | 16–20px |

### 7.3 Grid Structure

- Four columns for compact desktop card collections where content fits.
- Three columns for standard program or course cards.
- Two columns for split-content sections.
- One column for narrow mobile layouts.

Select the grid according to content density and available width rather than fixed device labels alone.

### 7.4 Layout Rules

- Align section headings to a consistent content container.
- Avoid unnecessary nested containers.
- Maintain predictable card widths.
- Prevent horizontal overflow.
- Keep related content visually grouped.
- Balance spacing between text and supporting imagery.

---

# Part VIII — Border Radius System

## 8. Border Radius Tokens

| Token | Value | Usage |
|---|---:|---|
| `radius-sm` | 6px | Small controls |
| `radius-md` | 10px | Buttons and inputs |
| `radius-lg` | 16px | Cards |
| `radius-xl` | 24px | Large panels |
| `radius-full` | 9999px | Pills and circular elements |

Avoid mixing unrelated radius values across similar components.

---

# Part IX — Shadow and Elevation System

## 9. Elevation Levels

| Level | Purpose |
|---|---|
| None | Flat sections and standard elements |
| Low | Cards requiring gentle separation |
| Medium | Dropdown menus and floating panels |
| High | Modal dialogs, when needed |

### 9.1 Shadow Guidelines

- Prefer borders or background contrast for standard cards.
- Use shadows sparingly.
- Avoid excessive blur and glow effects.
- Maintain consistent elevation for similar components.
- Do not use heavy shadows solely for decoration.

---

# Part X — Button System

## 10. Button Variants

Buttons communicate actions clearly and maintain consistent visual behavior. The following labels are static examples; final colors and contrast require review.

- **Explore Programs**
- **Learn More**
- **Contact Us**

### 10.1 Primary Button

**Purpose:** Highlight the main action within a section.

Examples include Explore Our Programs, Contact Us, and Request Information.

Proposed styling: Deep Purple background, white text, medium border radius, and clear hover and focus states.

### 10.2 Secondary Button

**Purpose:** Support an alternative action without competing with the primary CTA.

Proposed styling: transparent or white background, Deep Purple text, and a visible border.

### 10.3 Text Button

**Purpose:** Provide lightweight navigation within cards or content sections.

Examples include View Course Details, Learn More, and Explore Category.

### 10.4 Button States

Every interactive button should account for:

- Default
- Hover
- Focus
- Active
- Disabled
- Loading, where applicable

### 10.5 Button Guidelines

- Use clear action labels.
- Maintain consistent heights and padding.
- Avoid excessive button variants.
- Provide visible keyboard focus.
- Use links for navigation and buttons for actions.
- Ensure touch targets are comfortably usable.

---

# Part XI — Card System

## 11. Card Types

Cards organize signature programs, individual courses, learning methodology elements, and related content.

### 11.1 Program Card

Required elements:

- Category title
- Short description
- Supporting visual
- Navigation link

Use consistent padding and title styling, clear CTA placement, balanced content spacing, and responsive dimensions.

### 11.2 Course Card

Required elements:

- Course title
- Short overview
- Program category
- View Course Details link

Include optional course metadata only when verified.

### 11.3 Feature Card

Feature cards may present Learning Approach elements, organizational differentiators, or corporate-solutions previews. Keep content concise and avoid unnecessary decoration.

### 11.4 Card Interaction

Interactive cards may use subtle hover transitions. Make the entire card clickable only when its interaction is clear and accessible.

---

# Part XII — Navigation Components

## 12. Desktop Header

The desktop header includes the logo, navigation links, Programs dropdown, and Contact CTA.

Design rules:

- Maintain a consistent height and clear spacing.
- Use readable labels and a visible active state.
- Make dropdown controls accessible.

### 12.1 Mobile Navigation

The mobile navigation includes the logo, menu toggle, collapsible menu, expandable Programs submenu, and Contact CTA.

Design rules:

- Avoid crowded navigation.
- Provide clear open and close states.
- Ensure touch-friendly spacing.
- Support keyboard accessibility.
- Keep navigation order consistent.

### 12.2 Breadcrumbs

Breadcrumbs use compact typography, clear separators, descriptive labels, links to parent pages, and a distinguishable current-page label.

---

# Part XIII — Form Design System

## 13. Forms

Forms should prioritize clarity, usability, and accessibility. The Contact Us form may include Full Name, Email Address, Organization, Inquiry Type, and Message.

### 13.1 Input Styling

- Minimum control height of approximately 44px.
- Consistent border radius and clear input borders.
- Visible labels and adequate internal padding.
- Clear focus indicators.

### 13.2 Form States

- Default
- Focused
- Valid
- Invalid
- Disabled

### 13.3 Validation Messages

Error messages should explain the problem clearly, appear near the relevant field, remain readable, avoid relying only on red, and be accessible to assistive technologies.

### 13.4 Prototype Form Behavior

The form should demonstrate validation without claiming that information has been transmitted. Confirmation must explicitly identify the interaction as a prototype demonstration.

---

# Part XIV — Accordion System

## 14. FAQ Accordions

Accordions may be used for FAQ sections.

Required elements are a question label, expand/collapse indicator, answer content, and visible focus state.

Expected behavior:

- Questions can be expanded and collapsed.
- Controls support keyboard interaction.
- Expanded states are communicated accessibly.
- Content remains readable on mobile devices.

Avoid unnecessary animation or overly complex transitions.

---

# Part XV — Iconography Guidelines

## 15. Iconography

Icons should support navigation and comprehension.

### 15.1 Icon Style

Use one consistent icon family with similar stroke widths, consistent proportions, clear visual meaning, and appropriate sizes.

### 15.2 Recommended Usage

Icons may support navigation controls, contact information, learning methodology features, program cards, form feedback, and accordions.

### 15.3 Icon Rules

- Avoid mixing unrelated icon styles.
- Do not use icons as the only label for important actions.
- Provide accessible labels for icon-only controls.
- Avoid unnecessary decorative icons.
- Keep icon colors consistent with the design system.

---

# Part XVI — Image and Illustration Guidelines

## 16. Visual Assets

Visual assets should reinforce the organization's learning and development identity.

### 16.1 Preferred Image Types

- Approved organizational photography
- Professional training environments
- Collaborative learning activities
- Relevant workplace interactions
- Consistent illustrations, where appropriate

### 16.2 Image Usage Rules

- Use approved and properly licensed assets.
- Avoid generic or misleading imagery.
- Maintain consistent aspect ratios for similar components.
- Optimize image sizes for web delivery.
- Provide meaningful alternative text.
- Avoid placing essential information inside images.

### 16.3 Image Presentation

Images should support rather than overwhelm content. Hero imagery should relate clearly to the main message. Program-card imagery should remain visually consistent across the five categories.

---

# Part XVII — Responsive Design Tokens

## 17. Breakpoints

These breakpoints are proposed implementation references and should be adjusted according to actual layout behavior.

| Breakpoint | Width | Typical layout |
|---|---:|---|
| Small mobile | Below 480px | Single column |
| Mobile | 480–767px | Single column |
| Tablet | 768–1023px | Adaptive grid |
| Desktop | 1024–1279px | Multi-column |
| Large desktop | 1280px and above | Wide container |

### 17.1 Responsive Rules

1. Design components to adapt to available space.
2. Avoid fixed widths that cause overflow.
3. Maintain readable text sizes.
4. Adjust card columns as needed.
5. Keep touch controls accessible.
6. Reduce excessive spacing on smaller screens.
7. Preserve the same content hierarchy across devices.

---

# Part XVIII — Motion and Interaction Guidelines

## 18. Motion

Animations should be subtle and purposeful.

### 18.1 Appropriate Motion

- Button hover transitions
- Dropdown opening and closing
- Accordion expansion
- Modal appearance
- Small interactive feedback

### 18.2 Motion Rules

- Prefer short transitions.
- Avoid excessive scroll animations.
- Do not animate essential content unnecessarily.
- Respect reduced-motion preferences.
- Keep interactions understandable without animation.

### 18.3 Proposed Transition Durations

| Interaction | Suggested duration |
|---|---:|
| Button hover | 150–200ms |
| Card hover | 150–250ms |
| Dropdown | 150–200ms |
| Accordion | 200–250ms |
| Modal | 200–300ms |

These values are guidelines, not strict requirements.

---

# Part XIX — Accessibility Standards

## 19. Accessibility

The design system targets WCAG 2.2 Level AA where applicable.

### 19.1 Typography

- Maintain readable font sizes.
- Use appropriate line spacing.
- Preserve readability during zoom.
- Avoid text embedded in images.

### 19.2 Colors

- Meet relevant contrast requirements.
- Do not communicate meaning using color alone.
- Keep focus indicators visible.

### 19.3 Navigation

- Support keyboard access.
- Use semantic navigation structures.
- Provide clear active and focus states.
- Avoid hover-only functionality.

### 19.4 Forms

- Associate labels with fields.
- Provide understandable validation messages.
- Maintain logical keyboard navigation.
- Clearly identify required inputs.

### 19.5 Responsive Accessibility

- Support content reflow.
- Avoid unnecessary horizontal scrolling.
- Maintain accessible interactive controls.
- Keep content usable when text is enlarged.

---

# Part XX — Component Naming Conventions

## 20. Proposed Component Inventory

The following is a conceptual component structure, not a required framework-specific folder arrangement.

```text
components/
├── layout/
│   ├── Header
│   ├── Footer
│   ├── Container
│   └── Section
├── navigation/
│   ├── DesktopNavigation
│   ├── MobileNavigation
│   ├── ProgramsDropdown
│   └── Breadcrumbs
├── ui/
│   ├── Button
│   ├── SectionHeading
│   ├── PageHero
│   ├── ProgramCard
│   ├── CourseCard
│   ├── FeatureCard
│   ├── Accordion
│   └── CTASection
└── forms/
    ├── FormField
    ├── SelectField
    ├── TextAreaField
    └── InquiryForm
```

The final implementation should remain lightweight and avoid abstractions that are not needed.

---

# Part XXI — Proposed Design Tokens

## 21. CSS Variables

The following CSS variables provide an initial example. Values should be reviewed and refined during implementation.

```css
:root {
  /* Brand colors — proposed */
  --color-brand-primary: #281052;
  --color-accent-mint: #55dfbe;
  --color-accent-orange: #ffbd59;
  --color-accent-coral: #ff6875;

  /* Neutral colors */
  --color-background: #ffffff;
  --color-background-muted: #f8f6fc;
  --color-surface: #ffffff;
  --color-text-primary: #171321;
  --color-text-secondary: #514b5d;
  --color-border: #e6e2eb;

  /* Typography */
  --font-heading: "Outfit", sans-serif;
  --font-body: "DM Sans", sans-serif;

  /* Spacing */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-6: 24px;
  --space-8: 32px;
  --space-12: 48px;
  --space-16: 64px;
  --space-20: 80px;
  --space-24: 96px;

  /* Border radius */
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 16px;
  --radius-xl: 24px;

  /* Layout */
  --container-main: 1200px;
  --container-wide: 1320px;
  --container-reading: 760px;

  /* Motion */
  --transition-fast: 150ms ease;
  --transition-normal: 200ms ease;
}
```

---

# Part XXII — Design Consistency Checklist

## 22. Page Review

Before a page is considered visually complete, verify that:

- [ ] Approved colors and design tokens are used.
- [ ] Typography follows the defined hierarchy.
- [ ] Spacing is consistent.
- [ ] Page content aligns with the main container.
- [ ] Buttons use approved variants.
- [ ] Cards use consistent styling.
- [ ] Navigation follows the shared design.
- [ ] Form elements use consistent states.
- [ ] Images follow the asset guidelines.
- [ ] Responsive layouts work correctly.
- [ ] Accessibility requirements are considered.
- [ ] Decorative effects do not overwhelm content.

---

# Part XXIII — Design System Acceptance Criteria

## 23. Ready for Frontend Implementation

The proposed design system will be considered ready for frontend implementation when:

1. The visual direction is reviewed.
2. Brand colors are confirmed or explicitly approved for prototype use.
3. Typography choices are finalized.
4. Spacing and layout conventions are defined.
5. Reusable component styles are documented.
6. Button, card, form, and navigation states are specified.
7. Responsive behavior is established.
8. Accessibility requirements are incorporated.
9. The component inventory supports the page blueprint.
10. The system remains practical for the project's limited prototype scope.

---

# Part XXIV — Outstanding Design Decisions

## 24. Pending Review

| Item | Proposed direction | Status |
|---|---|---|
| Primary color | Deep Purple | Pending approval |
| Accent colors | Mint, Orange, Coral | Pending approval |
| Heading font | Outfit | Pending approval |
| Body font | DM Sans | Pending approval |
| Logo usage | Official organizational logo | Pending asset review |
| Program imagery | Consistent approved visuals | Pending selection |
| Card style | Minimal, softly rounded | Proposed |
| Button style | Solid primary and outlined secondary | Proposed |
| Section backgrounds | White and Off-White | Proposed |
| Animation style | Subtle and functional | Proposed |

---

# Part XXV — Conclusion

The proposed design system establishes a consistent visual and component foundation for The Learning Lab's website redevelopment.

By defining shared standards for colors, typography, spacing, layout, navigation, cards, forms, and interactive elements, it will support a cohesive user experience across the proposed pages. It also provides a practical reference for frontend development and helps reduce inconsistent styling and unnecessary duplication.

The final visual implementation should preserve the organization's approved brand identity while adapting suitable presentation concepts from the provided HTML prototype.

The next document, **08 — UI/UX Guidelines**, will define how these components should behave, including navigation, responsive interactions, accessibility, user feedback, and interface consistency.

**End of Document — 07-design-system.md.**
