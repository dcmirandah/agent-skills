---
name: drawio
description: Analyzes repositories to generate text overviews and Draw.io diagrams, or redesigns existing diagrams.
---

# Drawio

## Workflows

### 1. Repo Analysis
* **Scan**: Identify files, stack, and CI/CD.
* **Summary**: 3-7 sentences on purpose/outputs.
* **Diagram**: Map components, flows, and dependencies.

### 2. Diagram Redesign
* **Parse**: Extract lanes, steps, and legend.
* **Style**: Add shadows, modern colors (blue/green/orange/red), and whitespace.
* **Refine**: Use optional swim lanes and numbered steps (w/ emojis) for clarity.

## Specs
* **Typography**: Sans-serif (Helvetica), 18pt+.
* **Legend**: Required guide for colors/shapes/emojis.
* **Format**: marp-ready SVG.

## Output
* `<name>.drawio.svg` + `<name>.drawio`
