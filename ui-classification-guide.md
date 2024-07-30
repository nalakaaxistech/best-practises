# UI Component Classification Guide

## 🎯 Abstract

This guide aims to clarify the classification of components into atoms, molecules, organisms, etc., following the Atomic Design principle.

## 🔍 Scope

This guideline helps UX designers and UI developers understand and apply component classification in the atomic design hierarchy.

## 🧬 Atomic Design

Atomic design is a method for developing complex interfaces in a consistent, modular, and expandable manner. Components are categorized into "atoms", "molecules", "organisms", "templates", and "pages" in a structured and hierarchical way.

### Atoms

- Smallest, indivisible components of the design system
- Often raw HTML elements or abstract elements like colors, fonts, animations (design tokens)

### Molecules

- Simple combinations of atoms
- Represent groups of informational or functional atoms
- Can be simple or complex, but simpler molecules increase reusability

### Organisms

- Groups of connected molecules serving a specific purpose or function
- More complex than molecules

| Fact        | Molecules                                          | Organisms                    |
| ----------- | -------------------------------------------------- | ---------------------------- |
| Composition | Atoms + molecules                                  | Molecules + atoms            |
| Modularity  | Self-contained, relatively simple functional units | Larger and complex           |
| Reusability | Versatile, can be reused across different sections | Self-sufficient, independent |
| Scope       | Specific UI feature or interaction                 | Entire section of the UI     |

### Templates

- Composed of organisms, molecules, and atoms
- Define structure and interactions with placeholder content
- Provide context to molecules and organisms
- Resemble final wireframes by defining layout/section of a page

### Pages

- Specific instances of templates with real content
- Highest level in the atomic design hierarchy
- Represent real, specific pages of an application with real content and user interactions
- Act as a testing ground for refining lower-level components

## 🤔 How to Decide Component Classification

Use these questions to determine the classification of a component:

### Atoms

- Is it a basic UI element?
- Can it function independently without losing its meaning?
- Is it a raw HTML element?
  - Examples: button, input field, icon, label

### Molecules

- Does it consist of multiple atoms?
- Is it a small, self-contained component?
- Does it represent a simple interaction or feature?
  - Examples: search bar, tags, form fields with labels, datepicker, checkbox group

### Organisms

- Does it consist of molecules and/or atoms?
- Does it form a distinct section of the UI?
- Does it serve a specific purpose or function within a larger context?
  - Examples: header, footer, product card

### Templates

- Does it define the layout of a page or a section?
- Does it place organisms within a specific context?
- Does it lack real content but define the structure and placement of elements?
  - Examples: Product details, Blog post, Settings

### Pages

- Is it a specific instance of a template?
- Does it contain real content and data?
- Does it represent a complete, interactive interface that users can engage with?

## 🎨 Design Tokens

Design tokens are considered atoms and are centrally defined assets responsible for the appearance of the website. They allow for creating uniform, consistent user interfaces:

- Colors
- Typography
- Distances and sizes
- Shape characteristics (round corners, radius)
- Icons
- Animations
