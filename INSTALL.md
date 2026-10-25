# Installation (Windows 7 SP1 x64)

## Prerequisites

- **Windows 7 SP1 (64-bit)**
- **VxKex NEXT** — Windows 7 API extensions
  Download from: https://github.com/YuZhouRen86/VxKex/releases
  Use version **1.1.2.x or newer** (older versions have DXGI bugs)

## Step 1: Install VxKex NEXT

1. Download the x64 installer from the link above
2. Run it as administrator
3. Reboot when prompted

**Verify:** Open `C:\Program Files\VxKex\KexCfg.exe` — it should open without errors.

## Step 2: Extract PdfCraft

1. Extract `pdfcraft-win7-x64-portable.zip` to any folder
   - Recommended: `C:\Program Files\PdfCraft-Win7\` or `C:\Users\<you>\Desktop\PdfCraft\`
2. Verify the folder contains:
   - `pdfcraft.exe`
   - `pdfium.dll`
   - `run_pdfcraft.bat`
   - `NOTICE`, `LICENSE-*`, `PDFium-LICENSE.txt`
   - `README-WIN7.md`, `INSTALL.md`

## Step 3: Enable VxKex on the executable

1. Right-click `pdfcraft.exe` → **Properties**
2. Go to the **VxKex** tab
3. Check **"Enable VxKex for this program"**
4. (Optional, but recommended) Set **"Report a different version of Windows"** to **Windows 10**
5. Click **OK**

**Why:** VxKex provides missing Windows 8+ APIs that PdfCraft needs (e.g. `GetSystemTimePreciseAsFileTime`).

## Step 4: Run PdfCraft

**Double-click `run_pdfcraft.bat`** (not `pdfcraft.exe` directly).

The `.bat` file sets `PDFCRAFT_RENDERER=gl` before launching, so PdfCraft uses OpenGL instead of wgpu/DXGI. On Windows 7 with VxKex, DXGI is not fully emulated, and wgpu would fail silently.

If PdfCraft opens, you're done.

## Create a desktop shortcut (optional)

1. Right-click `run_pdfcraft.bat` → **Send to** → **Desktop (create shortcut)**
2. Rename the shortcut to `PdfCraft`
3. (Optional) Right-click → **Properties** → **Change Icon** → browse to `pdfcraft.exe`

---

## Troubleshooting

### Nothing happens when I double-click

- **Check:** Did you enable VxKex on `pdfcraft.exe`? (Step 3)
- **Check:** Are you running `run_pdfcraft.bat`, not `pdfcraft.exe`?
- **Check:** Is `pdfium.dll` in the same folder as `pdfcraft.exe`?

### "The procedure entry point ... could not be located in ..."

- VxKex is not enabled, or an older version is installed.
- Update VxKex NEXT to the latest version.

### PdfCraft opens but crashes when I open a PDF

- Check `pdfium.dll` version — it must match the architecture (x64)
- Verify the DLL is not corrupt: its size should be ~10 MB

### Printing doesn't work

- **Use:** Microsoft XPS Document Writer (built-in on Win7) to verify the pipeline
- **Or:** a virtual PDF printer like PDFCreator or Bullzip
- The print dialog may overflow on some window sizes — click **booklet** or another option with fewer settings to bring the Print button back into view

### I want to see VxKex's log

1. Open `C:\Program Files\VxKex\KexCfg.exe`
2. Enable **Logging**
3. Run PdfCraft again
4. Check `%LOCALAPPDATA%\VxKex\Logs\` for log files

Look for lines containing `failed`, `not found`, or `DXGI` — those are the meaningful ones.

---

## Uninstall

1. Delete the PdfCraft folder
2. (Optional) Uninstall VxKex from **Control Panel → Programs**
3. (Optional) Delete `%LOCALAPPDATA%\VxKex\` if you enabled logging
