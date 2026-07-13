# OME-Zarr Converter

A simple macOS app that converts microscopy images (`.tif`, `.nd2`, `.czi`) to **OME-Zarr** — the open standard for cloud-friendly, multidimensional bioimaging data.

![OME-Zarr Converter screenshot](docs/screenshot.png)

## Download

→ **[Download the latest version](https://github.com/serg-bg/ome-zarr-converter/releases/latest)**

Look under **Assets** for `OME-Zarr-Converter-mac.zip`. Double-click to unzip, then drag `OME-Zarr Converter.app` into your **Applications** folder.

## Verify your download

The release page publishes a SHA256 hash for `OME-Zarr-Converter-mac.zip`. After downloading, confirm the file is intact:

```bash
shasum -a 256 OME-Zarr-Converter-mac.zip
```

Compare the output to the hash listed on the [release page](https://github.com/serg-bg/ome-zarr-converter/releases/latest). They should match exactly.

## First launch (one-time)

macOS may block the app the first time because it isn't signed by a paid Apple Developer ID:

1. Right-click `OME-Zarr Converter.app` → **Open**
2. Click **Open** in the warning dialog
3. From now on, double-click works normally

## How to use

1. **Drag** a `.tif`, `.nd2`, or `.czi` file (or a folder of them) into the window
2. Pick (or accept) an **Output folder**
3. Click **Convert**

When conversion finishes, each file gets a row in the **Results** panel.

### Advanced options

Click **▶ Advanced** to set:

- **Voxel spacing (µm)** — auto-detected from `.nd2` / `.czi` metadata; falls back to `1.0` µm for `.tif` if you leave it blank
- **Treat input as labels / segmentation** — uses subsampling instead of mean-pooling when building the pyramid (preserves integer label IDs)
- **Channel** — pick a single channel from a multi-channel file (0-indexed; default: 0)
- **Pyramid levels** — how many resolution levels to generate (default: 4)

## Viewing in napari (optional)

The app can launch [napari](https://napari.org) on the converted file.

**First time only:** click **⚠ Install napari (first time only)** in the Results panel. A Terminal window opens and installs napari with [`uv`](https://github.com/astral-sh/uv). Wait for it to print `napari installed` and then close the window.

After that, the **Open in napari** button next to each result launches a separate napari window:

- The image opens in **2D**. Scroll to navigate slices. Zoom in to fetch higher-resolution tiles automatically (multiscale).
- Click the **cube icon** at the bottom-left of napari to switch to **3D volume** view.
- The **3D volume resolution** dropdown in the converter controls which pyramid level the 3D view uses. *Full res* is sharpest but heaviest; *Half / Quarter / Eighth* are progressively lighter for snappier rotation.

### Open an existing OME-Zarr (no conversion needed)

Already have a converted `.ome.zarr`? You can view it in napari without reconverting:

![Open existing OME-Zarr dialog](docs/open-existing-dialog.png)

1. Click **Open existing OME-Zarr in napari…**, or **drag an `.ome.zarr` folder** onto the drop area.
2. In the picker, choose the **3D volume resolution** — the list shows the pyramid levels actually present in your file, with their dimensions.
3. *(Optional)* overlay a **labels / segmentation** store. If a matching `<name>_labels.ome.zarr` sits next to the image, it's filled in automatically; otherwise click **Add labels…**. You can toggle **Smooth 3D labels** and a **Contour** width.
4. Click **Open in napari** — same behavior as above (2D multiscale, your chosen level in 3D).

#### Editing labels (paint and save back)

Tick **Edit labels** to load the labels at full resolution as an **editable** layer instead of a read-only overlay. Paint and correct segmentation directly in napari with the brush and fill tools, then click **Save labels** (a button added to napari's right-hand dock) to write the corrected labels back to the store — the multi-resolution pyramid is regenerated automatically, so the saved store stays consistent at every zoom level.

- Works whether you're correcting an **existing** labels store or starting from a **blank** layer — leave the labels overlay empty to annotate from scratch, and a new `<image>_labels.ome.zarr` is created next to the image.
- Editing loads the full-resolution labels into memory; volumes too large to fit are refused with a clear message rather than crashing.

Single-channel 3D (ZYX) volumes are supported. Multichannel or time-series stores aren't supported yet — you'll get a clear message rather than a silent failure.

## System requirements

- macOS 12 or later (Apple Silicon or Intel)
- ~250 MB free disk space for the app
- Internet connection on first napari install

## License

Copyright (C) 2026 Sergio Bernal-Garcia.

Licensed under the [GNU General Public License v3.0](LICENSE) (GPL-3.0). You are free to use, modify, and redistribute under the terms of that license. Source code is available on request for at least 3 years from the date of each binary release; see the release notes for source-request details.

## Author

Sergio Bernal-Garcia
