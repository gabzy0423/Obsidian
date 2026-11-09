---
title: "04 — Information Architecture"
project: "The Learning Lab Website Redesign and Redevelopment"
document_type: "Website Information Architecture"
status: "Proposed — For Review"
primary_reference: "https://thefoundrylab.com/"
secondary_reference: "Provided HTML Concept Prototype"
scope: "Public-Facing Corporate Website Prototype"
---

# 04 — Information Architecture

## The Learning Lab — Website Redesign & Redevelopment

> [!abstract] Document Information
> **Document Type:** Website Information Architecture
> **Project Status:** Proposed — For Review
> **Primary Reference:** [thefoundrylab.com](https://thefoundrylab.com/)
> **Secondary Reference:** Provided HTML Concept Prototype
> **Project Scope:** Public-Facing Corporate Website Prototype

---

## 1. Document Overview

This document defines the proposed information architecture for The Learning Lab's website redevelopment.

Information architecture refers to the organization, structure, labeling, and relationships of information within a website. It determines how content is grouped, how pages connect, and how visitors locate relevant information.

The redevelopment will consolidate the existing website's organizational information, learning methodology, signature training programs, and corporate learning solutions with selected design concepts from the provided HTML prototype.

The proposed architecture aims to establish a clear, scalable, and user-friendly website structure without introducing unnecessary pages, duplicate information, or complex backend functionality.

This document will serve as the structural reference for content planning, UI/UX design, and frontend prototype development.

---

## 2. Information Architecture Objectives

The proposed information architecture aims to:

1. Establish a clear hierarchy for all essential website pages.
2. Centralize the organization's signature training programs.
3. Organize course information into consistent category and detail pages.
4. Improve navigation between company information, programs, solutions, and contact pages.
5. Consolidate relevant content from the existing website and HTML prototype.
6. Reduce unnecessary content duplication.
7. Support intuitive navigation across desktop, tablet, and mobile devices.
8. Provide a maintainable structure for the interactive website prototype.

---

## 3. Information Architecture Principles

### 3.1 Content First

The website structure must be based on the organization's verified content and visitor information needs.

Pages should not be created solely to accommodate visual design elements from the HTML prototype.

### 3.2 Clear Navigation

Navigation labels should be familiar, concise, and understandable.

Visitors should be able to identify where to find programs, company information, and contact details without unnecessary exploration.

### 3.3 Logical Content Grouping

Related information should be organized under a common parent page.

For example, individual training courses should belong to their corresponding program categories.

### 3.4 Consistency

Pages with similar purposes should follow consistent naming conventions, hierarchy, and navigation patterns.

### 3.5 Minimal Duplication

Important content may be summarized across several pages, but detailed information should have one primary location.

### 3.6 Scalable Structure

The architecture should accommodate additional approved courses or informational content without requiring a complete redesign.

Scalability in this project refers to content organization, not backend infrastructure.

---

## 4. Proposed Website Structure

The proposed website will have five primary navigation destinations.

| Primary Page | Purpose |
|---|---|
| Home | Introduce The Learning Lab and highlight its offerings |
| About Us | Present organizational identity and learning philosophy |
| Our Programs | Centralize signature training programs and courses |
| Our Solutions | Present verified corporate learning solutions |
| Contact Us | Provide contact information and inquiry options |

Supporting pages will be accessible through relevant navigation links, content sections, or the footer.

This structure keeps the main navigation concise while allowing visitors to access detailed information through a logical hierarchy.

---

## 5. Proposed Website Sitemap

The following sitemap represents the recommended page hierarchy.

```text
THE LEARNING LAB
│
├── Home
│
├── About Us
│   └── Our Learning Approach
│
├── Our Programs
│   │
│   ├── Personal Development
│   │   ├── Course 01
│   │   ├── Course 02
│   │   └── Additional Verified Courses
│   │
│   ├── Managing Work
│   │   ├── Course 01
│   │   ├── Course 02
│   │   └── Additional Verified Courses
│   │
│   ├── Managing Relationships
│   │   └── Verified Courses
│   │
│   ├── Leadership & Management
│   │   └── Verified Courses
│   │
│   └── Impacting Strategy
│       └── Verified Courses
│
├── Our Solutions
│
├── Contact Us
│
└── Supporting Pages
    ├── FAQs (Optional)
    ├── Privacy Policy (Conditional)
    └── Other Approved Informational Pages
```

The course entries shown above are structural placeholders. Official course titles must be confirmed against the existing website before the course pages are finalized.

---

## 6. Page Hierarchy

The website will follow a three-level hierarchy for its primary informational content.

### Level 1 — Primary Pages

These pages are directly accessible from the main navigation:

- Home.
- About Us.
- Our Programs.
- Our Solutions.
- Contact Us.

### Level 2 — Supporting and Category Pages

These pages provide more specific information within a primary content area.

Examples include:

- Our Learning Approach.
- Personal Development.
- Managing Work.
- Managing Relationships.
- Leadership & Management.
- Impacting Strategy.

### Level 3 — Detailed Content Pages

These pages contain detailed information about individual training courses.

Examples include:

- Personal Development → Individual Course.
- Managing Work → Individual Course.
- Leadership & Management → Individual Course.

This hierarchy allows visitors to move from general information to specific content without introducing unnecessary navigation levels.

---

## 7. Navigation Architecture

### 7.1 Global Header Navigation

The proposed desktop navigation will include:

| Element | Behavior |
|---|---|
| The Learning Lab Logo | Returns to homepage |
| Home | Opens homepage |
| About Us | Opens company information |
| Our Programs | Opens program directory |
| Our Solutions | Opens corporate learning solutions |
| Contact Us | Opens contact page |

A prominent Contact Us or Let's Talk CTA may be used as the final navigation item.

### 7.2 Programs Dropdown

The Our Programs navigation item may provide a dropdown menu:

```text
Our Programs
│
├── View All Programs
├── Personal Development
├── Managing Work
├── Managing Relationships
├── Leadership & Management
└── Impacting Strategy
```

The dropdown should provide direct access to the five learning categories while retaining the main Programs directory as the central entry point.

On desktop, the dropdown should support mouse and keyboard interaction.

On mobile, it should use an expandable navigation group.

### 7.3 About Us Navigation

The About Us page should provide access to the dedicated Learning Approach page through internal links.

A separate top-level navigation item for Learning Approach is not required initially. This keeps the global navigation compact.

### 7.4 Footer Navigation

The footer should provide secondary access to important website destinations.

**Company**

- Home.
- About Us.
- Our Learning Approach.

**Programs**

- Our Programs.
- Personal Development.
- Managing Work.
- Managing Relationships.
- Leadership & Management.
- Impacting Strategy.

**Contact**

- Contact Us.
- Verified contact information.

**Legal**

- Privacy Policy, if applicable.

Footer content should remain concise and avoid repeating entire page descriptions.

---

## 8. Proposed URL Structure

The redesigned website should use readable, descriptive, and consistent URLs.

| Page | Proposed URL |
|---|---|
| Home | `/` |
| About Us | `/about` |
| Our Learning Approach | `/about/learning-approach` |
| Our Programs | `/programs` |
| Personal Development | `/programs/personal-development` |
| Managing Work | `/programs/managing-work` |
| Managing Relationships | `/programs/managing-relationships` |
| Leadership & Management | `/programs/leadership-management` |
| Impacting Strategy | `/programs/impacting-strategy` |
| Individual Course | `/programs/{category}/{course-slug}` |
| Our Solutions | `/solutions` |
| Contact Us | `/contact` |
| FAQs, if needed | `/faq` |
| Privacy Policy, if needed | `/privacy-policy` |

### URL Guidelines

1. Use lowercase letters.
2. Separate words with hyphens.
3. Avoid unnecessary abbreviations.
4. Keep URLs descriptive.
5. Maintain consistent parent-child relationships.
6. Avoid duplicate routes for the same content.
7. Preserve existing URL mappings for future redirects if the prototype proceeds to production.

The proposed URLs are not yet final production routes.

---

## 9. Content Hierarchy and Ownership

Each major content type should have a primary location.

| Content Type | Primary Location | Secondary Presentation |
|---|---|---|
| Organization Introduction | About Us | Homepage summary |
| Learning Philosophy | Our Learning Approach | Homepage and About previews |
| Signature Program Categories | Our Programs | Homepage cards |
| Individual Courses | Course Detail Pages | Category page cards |
| Corporate Learning Solutions | Our Solutions | Homepage preview |
| Contact Information | Contact Us | Footer |
| FAQs | Relevant pages or FAQ page | Selected homepage questions |

This approach ensures that detailed content is maintained in one primary location while allowing visitors to discover it from multiple entry points.

---

## 10. Content Consolidation Mapping

The redevelopment will map content from both references into the proposed website structure.

| Content Source | Existing or Prototype Element | Proposed Destination | Decision |
|---|---|---|---|
| Existing Website | Organization introduction | Home / About Us | Improve |
| Existing Website | Company background | About Us | Preserve |
| Existing Website | Four-element learning methodology | Our Learning Approach | Preserve and organize |
| Existing Website | Five signature categories | Our Programs | Consolidate |
| Existing Website | Course descriptions | Individual Course Pages | Standardize |
| Existing Website | Contact information | Contact Us / Footer | Preserve and verify |
| HTML Prototype | Hero design | Homepage | Adapt |
| HTML Prototype | Program cards | Home / Our Programs | Adapt |
| HTML Prototype | Learning process section | Our Learning Approach | Align with official methodology |
| HTML Prototype | Tailored learning solutions | Our Solutions | Validate |
| HTML Prototype | FAQ accordion | Relevant pages | Adapt |
| HTML Prototype | Inquiry modal | Contact interface | Adapt |
| HTML Prototype | HRIS concept | Not included by default | Defer |
| HTML Prototype | CADS concept | Not included by default | Defer |

This mapping prevents the redevelopment from becoming a collection of unrelated sections.

---

## 11. Signature Program Architecture

The five signature learning categories will form the core of the website's program organization.

### 11.1 Programs Directory

The Our Programs page will act as the parent page for all signature learning categories.

Its purpose is to introduce the available training areas and guide visitors toward the category most relevant to their needs.

### 11.2 Category Pages

Each category will have a dedicated page containing:

- Category title.
- Category overview.
- Relevant course listings.
- Course navigation.
- Related program links.
- Contact CTA.

### 11.3 Individual Course Pages

Each verified course will have a dedicated detail page using a standardized template.

The template may include:

- Official course title.
- Program category.
- Course overview.
- Learning objectives.
- Key topics.
- Target participants.
- Expected outcomes.
- Additional verified course details.
- Related courses.
- Contact CTA.

Optional fields should only appear when reliable information is available.

### 11.4 Course Classification Rules

Each course should be assigned to its official category.

Courses should not be duplicated across multiple category pages unless the organization confirms that they belong to more than one category.

Related courses may be recommended through internal links without duplicating their content.

---

## 12. User Journey Architecture

The website should support different visitor goals.

### 12.1 Journey A — Discover Training Programs

**User Goal:** Find a suitable professional development course.

```text
Homepage
   ↓
Our Programs
   ↓
Select Category
   ↓
Browse Courses
   ↓
View Course Details
   ↓
Contact Us
```

**Expected Outcome:** The visitor understands the available course and knows how to request more information.

### 12.2 Journey B — Learn About the Organization

**User Goal:** Understand The Learning Lab's identity and training approach.

```text
Homepage
   ↓
About Us
   ↓
Our Learning Approach
   ↓
Explore Programs
   ↓
Contact Us
```

**Expected Outcome:** The visitor gains confidence in the organization's approach and can explore relevant offerings.

### 12.3 Journey C — Explore Corporate Learning Solutions

**User Goal:** Identify training opportunities for an organization.

```text
Homepage
   ↓
Our Solutions
   ↓
Review Available Solutions
   ↓
Explore Related Programs
   ↓
Contact Us
```

**Expected Outcome:** The visitor understands the organization's relevant services and can initiate an inquiry.

### 12.4 Journey D — Direct Course Discovery

**User Goal:** Access a specific course from a direct link or search result.

```text
Individual Course Page
   ↓
Review Course Information
   ↓
Explore Related Courses
   ↓
Contact Us
```

**Expected Outcome:** The visitor can understand the course without needing to visit the homepage first.

---

## 13. Internal Linking Strategy

Internal links should connect related content and support visitor navigation.

### 13.1 Homepage Links

The homepage should link to:

- About Us.
- Our Programs.
- Five signature categories.
- Our Learning Approach.
- Our Solutions.
- Contact Us.

### 13.2 Category Page Links

Each category page should link to:

- Parent Programs directory.
- Individual courses.
- Related categories, where relevant.
- Contact Us.

### 13.3 Course Detail Links

Each course detail page should link to:

- Parent category.
- Programs directory.
- Related courses.
- Contact Us.

### 13.4 Solutions Page Links

The Our Solutions page should link to:

- Relevant signature programs.
- Learning Approach.
- Contact Us.

### 13.5 Link Consistency

Internal links should use descriptive labels rather than vague text such as “Click Here.”

Examples include:

- Explore Leadership Programs.
- View Course Details.
- Learn About Our Approach.
- Contact Our Team.

---

## 14. Breadcrumb Navigation

Breadcrumbs should be used on deeper pages to communicate location within the website hierarchy.

**Example — Category Page**

```text
Home > Our Programs > Managing Work
```

**Example — Course Detail Page**

```text
Home > Our Programs > Managing Work > Course Title
```

Breadcrumbs should:

- Reflect the actual page hierarchy.
- Provide links to parent pages.
- Use concise labels.
- Remain readable on mobile devices.
- Avoid unnecessary duplication of the primary page title.

Breadcrumbs are not required on the homepage.

---

## 15. Responsive Navigation Architecture

### 15.1 Desktop Navigation

The desktop header should display the primary navigation horizontally.

The Our Programs menu may include a dropdown for the five signature categories.

The contact CTA should remain visually identifiable.

### 15.2 Tablet Navigation

Navigation should adapt based on available screen width.

The design may transition to a collapsible menu when the horizontal navigation no longer fits comfortably.

### 15.3 Mobile Navigation

The mobile header should include:

- Website logo.
- Menu toggle.
- Expandable navigation.
- Programs submenu.
- Contact link.

The mobile menu should support clear open and close states.

### 15.4 Accessibility Requirements

Navigation should support:

- Keyboard interaction.
- Visible focus indicators.
- Clear link labels.
- Accessible menu controls.
- Appropriate touch target sizes.
- Predictable navigation behavior.

Dropdown menus should not rely exclusively on hover interaction.

---

## 16. Search and Filtering Strategy

The initial prototype does not require a complex search system.

Because the website contains a defined set of signature program categories, category-based browsing will serve as the primary discovery method.

A simple client-side course filter may be introduced if the verified course inventory supports it.

Potential filters include:

- Program category.
- Course title keywords.

Any filtering interface should operate only on available prototype content.

A database-backed search engine is outside the current project scope.

---

## 17. Supporting Pages and Content

### 17.1 Frequently Asked Questions

FAQs may be presented within relevant pages rather than requiring a separate destination.

A standalone FAQ page should only be introduced if the volume of approved content justifies it.

### 17.2 Privacy Policy

A privacy policy page may be required depending on the website's final data collection and publishing arrangements.

For the prototype, any sample policy content should be clearly identified as a placeholder until approved.

### 17.3 Resources and Articles

A Resources or Insights section is not included in the initial core sitemap.

It may be added later if the organization provides sufficient approved content.

### 17.4 Unapproved Services

Additional concepts from the HTML prototype, including HRIS and CADS, should remain outside the core information architecture unless the organization confirms their inclusion.

---

## 18. Information Architecture Validation

The proposed structure should be evaluated against the following criteria.

| Criterion | Validation Question |
|---|---|
| Clarity | Can visitors understand the main navigation labels? |
| Discoverability | Can visitors locate relevant programs easily? |
| Consistency | Are related pages organized under the correct parent? |
| Completeness | Are all verified signature categories represented? |
| Efficiency | Can visitors reach important information without unnecessary steps? |
| Scalability | Can additional courses be added without restructuring the website? |
| Accessibility | Can visitors navigate using different devices and input methods? |
| Content Integrity | Is official information separated from proposed concepts? |

---

## 19. Information Architecture Acceptance Criteria

The proposed information architecture will be considered ready for detailed content planning when:

1. All essential pages are identified.
2. The primary navigation is approved.
3. The five signature categories are represented.
4. Course detail pages have a defined parent category.
5. Page names and URLs follow consistent conventions.
6. Important user journeys are documented.
7. Content ownership is clearly defined.
8. Unnecessary duplicate pages have been avoided.
9. Prototype-only concepts are not presented as confirmed services.
10. The structure supports responsive and accessible navigation.

---

## 20. Open Decisions

The following decisions require confirmation before final implementation.

| Decision | Proposed Default |
|---|---|
| Dedicated Learning Approach page | Include under About Us |
| Programs dropdown | Include five category links |
| Individual course pages | Create for verified courses |
| Dedicated FAQ page | Optional |
| Resources / Insights section | Defer until content is available |
| HRIS and CADS pages | Exclude pending approval |
| Course search | Optional client-side enhancement |
| Inquiry form | Frontend demonstration only |
| Final URL structure | Confirm during implementation planning |

These decisions should be reviewed before the page-by-page blueprint is finalized.

---

## 21. Document Conclusion

The proposed information architecture establishes a clear foundation for The Learning Lab's website redevelopment.

By organizing the website around company information, learning methodology, signature training programs, corporate solutions, and contact pathways, the structure supports a more intuitive and consistent visitor experience.

The architecture also provides a practical method for consolidating content from the existing website and selected elements from the provided HTML prototype.

The proposed sitemap, navigation, page hierarchy, and user journeys will guide the next stages of content planning, UI/UX design, and frontend prototype development.

The overall objective remains to create one modern, organized, and cohesive corporate website without introducing unnecessary system complexity.

---

**End of Document — 04 Information Architecture**
