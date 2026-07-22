# Al Madhi Carpentry — ERP

A single-file, offline-first Progressive Web App running the full purchase and sales operations of a wooden pallet manufacturing and trading business in Sharjah, UAE.

## Overview

Al Madhi ERP handles both sides of the business in one codebase, switching between a Purchase mode (navy/gold theme) and a Sales mode (teal/aqua theme) via a single toggle — same UI and logic, different data pointer and theme. It's built specifically for wood/pallet trading, not adapted from generic accounting software: KG/bag-based weight pricing, auto-filled last prices by supplier, a shared cheque register across both modules, bilingual (English/Urdu) documents, and FIFO-based receivables aging.

## Features

- Dashboard with unified Purchase + Sales overview
- Invoice creation and full invoice/record history
- Payments with invoice-wise allocation
- Customer/supplier statements and search
- Shared cheque register (pending/cleared tracking)
- Expenses module with salary tracking
- Delivery notes and quotations
- Global search, PIN + biometric app lock
- Party and user management
- Export/backup (Excel via SheetJS) and bilingual invoice/document printing

## Tech Stack

- **Core:** Pure HTML5, CSS3, vanilla JavaScript — no framework, no build step (~17,000 lines, single file)
- **Backend:** Firebase Realtime Database (cloud-first, ES module SDK)
- **Export:** jsPDF, jsPDF-AutoTable, SheetJS (xlsx)
- **Offline:** Local write queue with dual-signal connection detection, replayed on reconnect

## Architecture

### System Architecture

```mermaid
flowchart TD
    subgraph Clients["Client Devices"]
        A[Phone]
        B[Laptop]
        C[Tablet]
    end
    A --> D[Al Madhi ERP<br/>Offline-First PWA]
    B --> D
    C --> D
    D --> E{Mode Toggle}
    E -->|Purchase| F[Purchase Mode<br/>Navy/Gold Theme]
    E -->|Sales| G[Sales Mode<br/>Teal/Aqua Theme]
    F --> H[Shared Data<br/>Cheque Register - Expenses<br/>Party and User Management]
    G --> H
    H <--> I[(Firebase Realtime Database<br/>Cloud Sync)]
```

### Offline Sync Flow

```mermaid
flowchart TD
    A[User performs action<br/>e.g. save invoice] --> B[Write to Local Queue<br/>localStorage]
    B --> C{Connected?<br/>navigator.onLine +<br/>Firebase .info/connected}
    C -->|No| D[Action stays queued<br/>app fully usable offline]
    D --> C
    C -->|Yes| E[Replay queue in order]
    E --> F[(Firebase Realtime Database)]
    F --> G[Broadcast update<br/>to other connected devices]
    F -.->|Concurrent offline edits| H[Last-write-wins on replay<br/>no smart merge - known limitation]
```

See [`docs/DOCUMENTATION.md`](docs/DOCUMENTATION.md) for the full data model and sync details.

## Screenshots

<table>
<tr>
<td><img src="Screenshot/screenshot-01.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-02.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-03.jpg" width="280"/></td>
</tr>
<tr>
<td><img src="Screenshot/screenshot-04.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-05.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-06.jpg" width="280"/></td>
</tr>
<tr>
<td><img src="Screenshot/screenshot-07.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-08.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-09.jpg" width="280"/></td>
</tr>
<tr>
<td><img src="Screenshot/screenshot-10.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-11.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-12.jpg" width="280"/></td>
</tr>
<tr>
<td><img src="Screenshot/screenshot-13.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-14.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-15.jpg" width="280"/></td>
</tr>
<tr>
<td><img src="Screenshot/screenshot-16.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-17.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-18.jpg" width="280"/></td>
</tr>
<tr>
<td><img src="Screenshot/screenshot-19.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-20.jpg" width="280"/></td>
<td><img src="Screenshot/screenshot-21.jpg" width="280"/></td>
</tr>
</table>

## Documentation

Full technical and feature documentation (dual-mode architecture, Firebase sync, every module, form validation, keyboard shortcuts, known bug fixes): [`docs/DOCUMENTATION.md`](docs/DOCUMENTATION.md)
