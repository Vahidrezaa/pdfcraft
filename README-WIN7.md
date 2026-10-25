# PdfCraft for Windows 7 (unofficial fork)

An **unofficial Windows 7 port** of [PdfCraft](https://github.com/storytold/pdfcraft), the open-source PDF editor built with Rust and egui.

This fork exists because PdfCraft officially targets Windows 10/11 only. On Windows 7, the official binary fails with `Windows.Data.Pdf.dll is missing`, because that DLL belongs to the WinRT PDF API introduced in Windows 8.1.

## Status

- ✅ **Viewer, editing, printing work** on Windows 7 SP1 x64
- ⚠️ **Requires VxKex NEXT** (Windows 7 API extensions) — see [INSTALL.md](INSTALL.md)
- ⚠️ **OpenGL rendering only** (wgpu/DXGI not supported on Win7 via VxKex)
- ⚠️ **Printing is raster only** (PDFium backend, not vector)
- ✅ **64-bit only** (32-bit not yet tested)

## How it works

Two changes to the upstream build:

1. **`Windows.Data.Pdf` (WinRT) → PDFium**
   Upstream's `craft-print` uses `Windows.Data.Pdf` for printing. On Win7 that API does not exist. This fork patches `craft-print` to use PDFium instead (via our fork of [winprint.rs](https://github.com/vahidrezaa/winprint.rs)).

2. **wgpu/DXGI → OpenGL (glow)**
   VxKex cannot fully emulate `CreateDXGIFactory2`, so wgpu fails silently. PdfCraft already has a `glow` fallback; we set `PDFCRAFT_RENDERER=gl` via `run_pdfcraft.bat`.

## Installation

See [INSTALL.md](INSTALL.md).

## Download

Grab the latest `pdfcraft-win7-x64-portable.zip` from [Releases](../../releases).

## Building

Push to the `win7-support` branch. GitHub Actions builds automatically.

## Limitations

- **Printing quality is raster** — text is no longer searchable in the printed output. If you need vector printing, this fork is not for you.
- **High DPI printing** may look soft compared to the official vector output.
- **The print dialog may overflow** on some window sizes (upstream bug, not specific to this fork).
- **Not tested with all printer drivers** — Microsoft XPS Document Writer and virtual PDF printers are confirmed working.

## Licenses

This fork redistributes:

- **PdfCraft** (MIT OR Apache-2.0) — see `NOTICE`, `LICENSE-MIT`, `LICENSE-APACHE`
- **PDFium binaries** (MIT, Benoit Blanchon) — see `PDFium-LICENSE.txt`
- **winprint.rs** (BSD-3-Clause, alampy.com) — embedded in `pdfcraft.exe`, see upstream fork notices

See `NOTICE` for the full list of third-party components bundled with PdfCraft itself.

## Upstream

- PdfCraft: [storytold/pdfcraft](https://github.com/storytold/pdfcraft)
- winprint fork: [vahidrezaa/winprint.rs](https://github.com/vahidrezaa/winprint.rs)
- craft-libs fork: [vahidrezaa/craft-libs](https://github.com/vahidrezaa/craft-libs)
- This fork: [vahidrezaa/pdfcraft](https://github.com/vahidrezaa/pdfcraft)
