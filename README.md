# PCI CHD Flow Diagram Builder

A fully self-contained, browser-based tool for creating **PCI DSS Cardholder Data (CHD) flow diagrams**. Designed for security teams, QSAs, and compliance engineers who need to document, visualise, and communicate how payment card data moves through their environments.

No installation. No server. No dependencies. Open the HTML file in any modern browser and start diagramming.

---

## Why This Exists

PCI DSS requires organisations to maintain accurate data flow diagrams showing how cardholder data (CHD) enters, traverses, and exits their systems (Requirements 1.2.4 and 12.4). These diagrams are reviewed by QSAs during assessments and must clearly distinguish between in-scope and out-of-scope flows, encrypted vs. unencrypted transmission, and where sensitive data is stored.

Most diagramming tools (Visio, Lucidchart, draw.io) are general-purpose — they don't know what a tokenization vault is, they won't warn you when you draw an unencrypted PAN connection, and they can't tell you which PCI DSS requirement applies to the component you just placed.

This tool is built specifically for CHD flow documentation, with PCI-aware components, colour-coded data flow types, and inline compliance guidance.

---

## Features

### Interactive Canvas
- **Drag-and-drop placement** — click a component in the sidebar, then click the canvas to place it
- **Port-based connections** — hover any component to reveal four connection ports (N/S/E/W); drag from a port to another component to draw a connection
- **Click-to-connect mode** — use the Connect toolbar button for a click-click workflow without dragging
- **Smooth bezier routing** — connection lines automatically curve between ports for clean, readable diagrams
- **Pan and zoom** — drag the canvas background to pan; scroll to zoom in and out (15% to 400%)
- **Double-click to rename** — edit any component label inline
- **Right-click context menu** — quick access to edit or delete any element
- **Keyboard shortcuts** — `Delete`/`Backspace` removes selected items; `Escape` cancels any active mode

### PCI-Aware Components (27 types)

Components are organised into six categories matching common PCI DSS scoping language:

| Category | Components |
|---|---|
| **Actors** | Cardholder, Merchant |
| **Devices** | POS Terminal, ATM, Card Reader, Mobile Device, Web Browser, Kiosk |
| **Systems** | Payment App, Web Server, App Server, Payment Gateway, Load Balancer, API Server |
| **Security** | Firewall, WAF, IDS/IPS, HSM, Tokenization Vault, P2PE Device |
| **Storage** | CHD Database, Log Server, Backup System |
| **External** | Card Network, Acquiring Bank, Issuing Bank, PSP / Processor |

### Data Flow Types

Each connection is typed with a colour-coded line indicating what data is in transit:

| Type | Colour | Style | Notes |
|---|---|---|---|
| ⚠️ Unencrypted PAN | Red | Dashed | Triggers PCI violation warning (Req 4.2.1) |
| 🔒 Encrypted PAN | Blue | Solid | Guidance on TLS 1.2+ requirements |
| 🔐 Tokenized | Green | Solid | Notes on scope reduction |
| 🔄 Authorization | Purple | Solid | |
| 💰 Settlement | Amber | Solid | |
| ⬜ Out of Scope | Grey | Dashed | |
| → Generic Flow | Dark grey | Solid | For non-CHD connections |

### Inline PCI Guidance

The Properties panel surfaces relevant PCI DSS requirements based on what you've selected:
- Placing a **CHD Database** → reminds you of encryption at rest requirements (Req 3.5)
- Placing a **POS Terminal** or **Card Reader** → flags PTS validation and tamper protection (Req 9.9)
- Placing a **Firewall** → reminds you to document all rules (Req 1)
- Placing an **HSM** → references FIPS 140-2 validation requirements (Req 3.7)
- Drawing an **Unencrypted PAN** connection → flags the Req 4.2.1 violation
- Drawing an **Encrypted** connection → reminds you of TLS cipher suite requirements

### Save and Load
Diagrams can be saved to a `.json` file and reloaded later — preserving all components, connections, labels, positions, and canvas state. Use this to maintain versioned diagram files alongside your other compliance documentation.

### Export
- **SVG** — scalable vector format; ideal for embedding in reports or wikis
- **PNG** — 2× resolution raster image suitable for presentations and documents
- **PDF** — opens a print-ready page with diagram metadata; use browser Print → Save as PDF

---

## Usage

### Quickstart
1. Download `chd-flow-diagram-builder.html`
2. Open it in Chrome, Firefox, Edge, or Safari
3. A sample e-commerce CHD flow loads automatically as a reference
4. Click **⬜ New** to clear the canvas and start your own diagram

### Building a Diagram
1. Click a component type in the left sidebar to arm your cursor
2. Click on the canvas to place the component (click again to place another; press `Esc` to stop)
3. Hover a placed component to reveal its connection ports (small blue circles)
4. Drag from a port to another component to create a connection
5. With the connection selected, use the Properties panel to set the **Flow Type** and add a label
6. Double-click any component to edit its label; right-click for delete options

### Tips
- Use **⊡ Fit** to zoom the view to fit all placed components
- Components can be freely repositioned by dragging — connected edges re-route automatically
- Add notes to each component using the Properties panel (useful for capturing IP ranges, system names, or assessment notes)
- Save frequently using **💾 Save** to preserve your work

---

## Technical Notes

The tool is implemented as a single HTML file (~1,200 lines) with no external dependencies:
- **Rendering** — SVG-based canvas with a pan/zoom viewport transform
- **Connections** — cubic bezier curves with auto-routed control points based on port direction
- **Export** — SVG serialization for SVG/PNG export; native browser Print API for PDF
- **Save/Load** — JSON serialization of diagram state via the File API

Tested in Chrome 120+, Firefox 121+, Edge 120+, and Safari 17+.

---

## PCI DSS Compliance Context

This tool supports documentation for **PCI DSS v4.0.1** assessments. Diagrams produced should be reviewed against the following requirements:

- **Req 1.2.4** — Accurate network diagrams showing all connections to cardholder data
- **Req 4.2.1** — PAN protected with strong cryptography during transmission
- **Req 3.5** — Primary account number (PAN) secured wherever stored
- **Req 3.7** — Cryptographic key management procedures
- **Req 9.9** — Protection of point-of-interaction (POI) devices from tampering

> **Disclaimer:** This tool assists with the documentation of cardholder data flows. It does not validate, audit, or certify PCI DSS compliance. All diagrams should be reviewed by a Qualified Security Assessor (QSA) as part of a formal assessment.

---

## License

MIT — free to use, modify, and distribute.
