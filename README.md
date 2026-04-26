# CHDgen — PCI DSS Cardholder Data Flow Builder

CHDgen is a fully self-contained, browser-based tool for creating PCI DSS Cardholder Data (CHD) flow diagrams. Designed for security teams, QSAs, and compliance engineers who need to document, visualize, and communicate how payment card data moves through their environments.

No installation. No server. No dependencies. Open the HTML file in any modern browser and start diagramming.

---

## Why This Exists

PCI DSS requires organizations to maintain accurate data flow diagrams showing how cardholder data enters, traverses, and exits their systems (Requirements 1.2.4 and 12.4). These diagrams are reviewed by QSAs during assessments and must clearly distinguish between in-scope and out-of-scope flows, encrypted versus unencrypted transmission, and where sensitive data is stored.

Most diagramming tools (Visio, Lucidchart, draw.io) are general-purpose — they don't know what a tokenization vault is, they won't warn you when you draw an unencrypted PAN connection, and they can't tell you which PCI DSS requirement applies to the component you just placed.

CHDgen is built specifically for CHD flow documentation, with PCI-aware components, colour-coded data flow types, inline compliance guidance, and scope boundary visualization.

---

## Features

### Interactive Canvas

Components are placed by clicking a type in the sidebar, then clicking the canvas. Hover any placed component to reveal four connection ports (N/S/E/W); drag from a port to another component to draw a connection, or use Connect mode in the toolbar for a click-click workflow without dragging. Connections route automatically using orthogonal (right-angle) paths — L-bends when ports are perpendicular, Z-bends when they are parallel — and re-route in real time as components are moved.

Pan the canvas by dragging the background. Scroll to zoom between 15% and 400%, zooming toward the cursor position. The ± toolbar buttons zoom to the canvas center. Use ⊡ Fit to zoom the view to contain all placed components and zones.

### Undo / Redo

Every destructive or structural action — placing a component, drawing a connection, moving items, deleting, changing a flow type — is recorded in a 50-step history. `Ctrl+Z` undoes and `Ctrl+Y` (or `Ctrl+Shift+Z`) redoes. The toolbar buttons grey out when the stack is empty. An amber dot appears in the toolbar whenever there are unsaved changes.

### Multi-Select

Drag across empty canvas space to draw a rubber-band selection rectangle and select all components within it. Hold `Shift` and click individual components to add or remove them from the selection. Selected components can be moved together as a group or deleted all at once with `Delete`. The Properties panel summarises the selection count when multiple items are active.

### Copy, Paste, and Duplicate

`Ctrl+C` copies the current selection (single node or multi-select). `Ctrl+V` pastes with a grid-offset so pasted components don't land directly on top of their originals. `Ctrl+D` or right-click → Duplicate duplicates the active single component. Pasted components are automatically placed into a new multi-selection for immediate repositioning.

### Grid Snap

All placement and drag-end positions snap to a 22-pixel grid (matching the canvas dot grid). Arrow keys nudge the selected component or selection by one grid unit. The ⊞ Snap toolbar button toggles snapping on and off.

### Inline Label Editing

Double-click any component or zone to open a floating text input positioned over it and scaled to the current zoom level. Press `Enter` to commit or `Escape` to cancel. Right-click → Edit Label provides the same input from the context menu.

### CDE Boundary Zones

The sidebar's **Boundary Zones** section lets you draw labelled scope rectangles directly on the canvas. Drag to define the zone area; the result is a coloured, dashed-border region that sits below all components and does not block interaction with them.

| Zone | Colour | PCI Context |
|---|---|---|
| CDE | Red | All 12 PCI DSS requirement areas apply to everything inside |
| Connected to CDE | Amber | May be in scope depending on segmentation; discuss with your QSA |
| Out of Scope | Grey | Verify effective segmentation annually (Req 11.4.1) |

Zones are moveable (drag the header bar), renameable (double-click), and deletable (right-click). Zone-specific guidance appears in the Properties panel when a zone is selected.

### PCI-Aware Components (30 types)

Components are organised into six categories matching common PCI DSS scoping language.

| Category | Components |
|---|---|
| Actors | Cardholder, Merchant |
| Devices | POS Terminal, ATM, Card Reader, Mobile Device, Web Browser, Kiosk |
| Systems | Payment App, Web Server, App Server, Payment Gateway, Load Balancer, API Server |
| Security | NSC (Firewall), WAF, IDS/IPS, HSM, Tokenization Vault, P2PE Device, Encryption Proxy |
| Storage | CHD Database, Log Server, Backup System |
| External | Card Network, Acquiring Bank, Issuing Bank, PSP / Processor, Cloud Provider, TPSP / 3rd Party |

### Data Flow Types

Each connection carries a type that determines its colour and line style.

| Type | Colour | Style | Notes |
|---|---|---|---|
| ⚠️ Unencrypted PAN | Red | Dashed | Triggers Req 4.2.1 violation warning |
| 🔒 Encrypted PAN | Blue | Solid | Cipher suite selector; TLS guidance (Req 4.2.1) |
| 🔐 Tokenized | Green | Solid | Scope reduction guidance; vault segmentation note |
| 🔄 Authorization | Purple | Solid | Encryption and SAD logging guidance (Req 3.3.1) |
| 💳 Capture | Teal | Solid | Post-auth encryption and logging requirements (Req 10) |
| 💰 Settlement | Amber | Solid | Batch reconciliation and encryption guidance |
| ⛔ SAD | Dark red | Dashed | SAD sub-type selector; Req 3.3.1 violation check |
| ⬜ Out of Scope | Grey | Dashed | For flows outside the CDE |
| → Generic Flow | Dark grey | Solid | For non-CHD connections |

SAD flows targeting a storage component (CHD Database, Backup, Log Server) display a **Critical Violation** badge in the Properties panel identifying the specific storage type.

Encrypted PAN connections expose a cipher suite selector with common TLS 1.2/1.3 options plus a custom field.

### Inline PCI Guidance

The Properties panel surfaces relevant requirements based on what is selected.

| Component or Flow | Guidance surfaced |
|---|---|
| CHD Database | Encrypt at rest (Req 3.5); minimise stored data |
| POS Terminal, Card Reader | PTS validation and tamper protection (Req 9.3) |
| NSC (Firewall) | Document rules; review every 6 months (Req 1.2.2) |
| WAF | Active blocking mode; rule reviews (Req 6.4) |
| HSM | FIPS 140-2/140-3 Level 3 validation (Req 3.7) |
| Tokenization Vault | Vault segmentation; only the vault can de-tokenize |
| P2PE Device | PCI P2PE listing maximises scope reduction |
| Encryption Proxy | TLS termination scope and key management |
| Cloud Provider | Current AOC / Responsibility Matrix; inherited vs. shared controls (Req 12.8) |
| TPSP / 3rd Party | Written agreement (Req 12.8.2); annual compliance confirmation (Req 12.8.4) |
| Unencrypted PAN connection | Req 4.2.1 violation; encrypt or tokenize |
| Encrypted PAN connection | TLS 1.2+ requirements; disable weak ciphers |
| SAD connection | Req 3.3.1; pre-authorisation only; storage violation check |

### Narrative and Version Control

The 📝 Narrative button opens a modal where you can write a free-text description of the cardholder data environment — scope, security controls, third-party dependencies, assumptions, exclusions — alongside a version history table. Each version entry captures a version number, date, and change notes. Narrative content and version history are persisted in the saved JSON file and included in PDF exports.

### Save and Load

Diagrams are saved to a `.json` file and reloaded later, preserving all components, zones, connections, labels, positions, canvas state, narrative, and version history. The saved file is human-readable and suitable for version-controlling alongside other compliance documentation.

On load, the file is validated for structural integrity — unknown component types, missing coordinates, and dangling edge references are caught before they can corrupt the canvas state.

### Export

**SVG** — Scalable vector output; ideal for embedding in reports, wikis, or Confluence pages. Selection rings, ports, and interactive elements are stripped from the export.

**PNG** — 2× resolution raster image suitable for presentations and Word documents.

**PDF** — Opens a print-ready browser window containing the diagram, generation date, version badge, and the full narrative text if one has been written. Use browser Print → Save as PDF.

---

## Usage

### Quickstart

1. Download `CHDgen.html`
2. Open it in Chrome, Firefox, Edge, or Safari
3. A sample e-commerce CHD flow loads automatically, including a CDE boundary zone, as a reference
4. Click ⬜ New to clear the canvas and start your own diagram

### Building a Diagram

Start by drawing your scope boundaries. In the sidebar, under **Boundary Zones**, drag a CDE zone onto the canvas to represent your cardholder data environment. Add Connected or Out of Scope zones as needed to show your segmentation story.

Then place components. Click a type in the sidebar to arm the cursor, then click the canvas to place it inside or outside your zones. Press `Esc` when you're done placing that type. Drag placed components to reposition them — edges re-route automatically.

To connect components, hover a placed component to reveal its four port circles, then drag from a port to the target component. With the resulting connection selected, use the Properties panel to set the Flow Type and add a descriptive label (for example, "PAN (TLS 1.3)" or "Auth Request").

Double-click any component to edit its label inline. The Properties panel also accepts free-text notes per component — useful for IP ranges, system names, hostnames, or assessment notes.

### Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+Z` | Undo |
| `Ctrl+Y` / `Ctrl+Shift+Z` | Redo |
| `Ctrl+C` | Copy selection |
| `Ctrl+V` | Paste |
| `Ctrl+D` | Duplicate selected component |
| `Delete` / `Backspace` | Delete selected item(s) |
| `Arrow keys` | Nudge selected component(s) by one grid unit |
| `Escape` | Cancel active mode; deselect all |
| `Shift+click` | Add/remove component from multi-selection |

---

## Technical Notes

The tool is implemented as a single HTML file (~1,800 lines) with no external dependencies.

**Rendering** — SVG-based canvas with a pan/zoom viewport transform applied to a single `<g>` element. Zones, edges, and nodes render in separate sub-layers so zones always sit beneath connections and components.

**Edge routing** — Orthogonal (rectilinear) paths computed from source and target port directions. L-bend when ports are perpendicular (horizontal source, vertical target or vice versa); Z-bend through the midpoint when both ports face the same axis. Edge label backgrounds are sized using `SVGTextElement.getComputedTextLength()` for accurate fit regardless of font or emoji width.

**History** — Structural state (nodes, edges, zones) is JSON-serialised into a 50-entry ring buffer on every mutation. Drags snapshot state at start and only commit the entry if movement exceeds a 2px threshold, avoiding spurious history entries from clicks.

**Export** — The SVG clone strips interactive elements (ports, selection rings, hit rects, marquee overlays) before serialisation. PNG export uses a 2× Canvas with the SVG rendered via an `<img>` element. All Blob URLs are revoked after use.

**Save/Load** — Full diagram state serialised as JSON via the File API. On load, a validation pass checks node types, coordinate presence, and edge reference integrity before any state is mutated, with a descriptive error toast on failure.

Tested in Chrome 120+, Firefox 121+, Edge 120+, and Safari 17+.

---

## PCI DSS Compliance Context

This tool supports documentation for PCI DSS v4.0.1 assessments. Diagrams produced should be reviewed against the following requirements:

- **Req 1.2.4** — Accurate network diagrams showing all connections to cardholder data
- **Req 4.2.1** — PAN protected with strong cryptography during transmission
- **Req 3.3.1** — Sensitive authentication data (SAD) not retained after authorisation
- **Req 3.5** — Primary account number (PAN) secured wherever stored
- **Req 3.7** — Cryptographic key management procedures
- **Req 6.4** — Web-facing applications protected by a WAF
- **Req 9.3** — Protection of point-of-interaction (POI) devices from tampering
- **Req 11.4.1** — Penetration testing to verify segmentation controls annually
- **Req 12.8** — Management of third-party service providers with access to the CDE

---

> **Disclaimer:** CHDgen assists with the documentation of cardholder data flows. It does not validate, audit, or certify PCI DSS compliance. All diagrams should be reviewed by a Qualified Security Assessor (QSA) as part of a formal assessment.
