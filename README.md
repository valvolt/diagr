# TAM Editor — Small Diagram Editor

A single-file, client-side diagram editor for creating architecture and system diagrams. Define elements as text, get an auto-layouted SVG diagram instantly.

No build step, no server — just open `index.html` in a modern browser.

## Usage

Open `index.html` in your browser. The left column has four text panes; the right column shows the live SVG diagram.

### Assets

System components — apps, services, people. One per line:

```
id: label
```

- IDs starting with `H` render a stick-figure human icon (e.g. `H1: Alice`)
- IDs ending with `M` (length ≥ 2) render as "multiple" — a stacked visual with a shadow element behind (e.g. `AM: Cluster`)

```
H1: Alice (admin)
A1: Browser
A2: Web app
AM: App cluster
```

### Storages

Data stores rendered with rounded corners. One per line:

```
S1: Database
S2: Cache
SM: Replicated DB
```

IDs ending with `M` (length ≥ 2) also render as "multiple" (stacked visual).

### Inside (Containment)

Nest a child node inside a parent. One per line:

```
A2 in A3
S1 in A3
```

Parents auto-resize to fit their children. Cycles are detected and rejected. As you type, recognized IDs show their labels as inline grey ghost text.

### Connectors

All connections between nodes — both asset-to-asset (connectors) and asset-to-storage (flows) — go in a single pane. The editor auto-detects the type based on endpoint kinds.

Directed or bidirectional links with optional label after `:`:

```
H1 -> A1
A1 <-> A2 : https
A2 <-> S1 : jdbc
A1 -> S2 : write
```

- **Asset ↔ Asset**: rendered as orthogonal paths with TAM-style connector decorations (circle + arrowhead + label)
- **Asset ↔ Storage**: rendered as flow arrows (thin lines with small arrowheads); bidirectional flows use curved Bezier arcs

As you type, recognized IDs show their labels as inline grey ghost text.

## Features

- **Live preview** — diagram updates as you type (180ms debounce)
- **Auto layout** — grid-based layout with step-minimization scoring; hub nodes get permutation-optimized neighbor placement
- **Inline ID resolution** — Inside and Connectors panes show resolved labels as grey ghost text next to typed IDs
- **Auto ID** — type `:` at the start of a line in Assets or Storages to auto-insert the next available ID (e.g. `A4: `)
- **Click to color** — single-click a node to cycle through pastel colors; double-click to reset
- **Smart edges** — port selection minimizes arrow segments; straight lines preferred over L-shapes; curved paths for bidirectional flows
- **Fit to view** — the SVG viewBox adjusts automatically to content

## Import / Export

| Button | Format | Description |
|--------|--------|-------------|
| **Export** | `.txt` | All pane contents in a section-delimited text format (including node colors) |
| **Export SVG** | `.svg` | Raw SVG of the current diagram |
| **Export PNG** | `.png` | Rasterized PNG via Canvas |
| **Import** | `.txt` | Load a previously exported `.txt` file back into the editor |
| **Clear** | — | Reset all panes and the diagram (with confirmation) |

### Text format

Exported `.txt` files use section markers:

```
--- ASSETS ---
H1: Alice (admin)
A1: Browser

--- STORAGES ---
S1: Database

--- INSIDE ---
A1 in A2

--- CONNECTORS ---
H1 -> A1
A1 <-> S1 : jdbc

--- COLORS ---
A1: #dff7df
```

## Requirements

- A modern browser (Chrome, Firefox, Safari, Edge)
- No server, no build tools, no install
