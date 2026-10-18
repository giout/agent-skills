---
name: mockups
description: Implement navigable UI mockups from an existing feature implementation plan and provide their view routes. Use when asked to preview a planned feature in the current application.
---

# Feature Mockups

Read the feature implementation plan at `[PLAN_FILE]` and build the mock UI that would appear if the plan were implemented.

## Workflow

1. Read the plan and inspect the application's existing routes, layouts, components, and visual conventions.
2. Identify the planned screens, important UI states, and navigation needed to demonstrate the feature.
3. Implement the mock views using existing application patterns and representative mock data. Keep the work focused on the UI described in the plan; do not implement production backend behavior solely to support the mockup.
4. Make the views reachable through application routes and verify that they render and support the interactions needed to preview the plan.
5. Return the routes or full local URLs for each view, with a short explanation of what each shows and any material preview limitations.
