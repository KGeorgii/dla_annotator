# DLA Annotator

A single-file, browser-based bounding-box annotation tool for document layout analysis (DLA) of digitized periodical pages. No install, no server, no build step — open `dla-annotator.html` in any modern browser and start annotating.

Built to produce the training data for [vsesvit-layout-yolov8m](https://huggingface.co/kgeorgii/vsesvit-layout-yolov8m), a YOLOv8m layout detection model for Ukrainian Soviet-era periodicals.

---

## Features

- **Drag-and-drop PDF loading** — load one or multiple PDFs at once; all pages are listed in the left panel
- **8-class label taxonomy** — `journal_name`, `article_title`, `author_name`, `page_number`, `text_block`, `image`, `mixed_text`, `decorative_element`
- **Draw bounding boxes** by clicking and dragging directly on the page canvas
- **Select, highlight, and delete** individual annotations from the right panel or canvas
- **Per-page metadata** — journal name, year, issue number, source URL
- **Zoom controls** — keyboard (`+`/`-`) or buttons; zoom persists across page navigation
- **Keyboard shortcuts** for fast annotation (see below)
- **Three export formats**:
  - **JSONL** — HuggingFace Datasets-compatible, one record per page
  - **YOLO TXT** — normalized `class_id cx cy w h` format, ready for YOLOv8 training
  - **PESP Parquet** — instructions for merging with the [PESP corpus](https://huggingface.co/datasets/apjanco/pesp) via `datasets`
- **Import** — reload a previously exported JSONL file to resume annotation

---

## Usage

1. Download `dla-annotator.html`
2. Open it in any modern browser
3. Drag PDF files onto the window, or click **Load PDF(s)**
4. Select a label class (right panel or keys `1`–`8`)
5. Draw boxes by clicking and dragging on the page
6. Fill in the metadata fields (journal, year, issue, source URL)
7. Click **Save Page** or press `S` before navigating away
8. When done, click **Export ↓** to download your annotations

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `1` – `8` | Select label class |
| `←` / `→` | Previous / next page |
| `Del` / `Backspace` | Delete selected annotation |
| `Esc` | Deselect annotation |
| `+` / `-` | Zoom in / out |
| `S` | Save current page |

---

## Export Formats

### JSONL (HuggingFace)
Each line is a JSON object for one annotated page:
```json
{
  "filename": "issue_01_3",
  "journal": "My Journal",
  "year": "1932",
  "issue": "No. 1",
  "uri": "https://...",
  "yolo": "0 0.512 0.043 0.476 0.061\n4 0.501 0.312 0.890 0.420",
  "yolo_predictions": [{"label": "journal_name", "x": ..., "y": ..., "w": ..., "h": ...}]
}
```

### YOLO TXT
Standard YOLOv8 label format — normalized center-x, center-y, width, height:
```
0 0.512000 0.043000 0.476000 0.061000
4 0.501000 0.312000 0.890000 0.420000
```

### Class IDs

| ID | Class |
|----|-------|
| 0 | `journal_name` |
| 1 | `article_title` |
| 2 | `author_name` |
| 3 | `page_number` |
| 4 | `text_block` |
| 5 | `image` |
| 6 | `mixed_text` |
| 7 | `decorative_element` |

---

## Dependencies

- [PDF.js 3.11.174](https://mozilla.github.io/pdf.js/) — loaded from the Cloudflare CDN, no local install needed
- Everything else is vanilla HTML, CSS, and JavaScript

---

## Related

- **Model**: [kgeorgii/vsesvit-layout-yolov8m](https://huggingface.co/your-username/vsesvit-layout-yolov8m)
- **Dataset**: [kgeorgii/vsesvit-dla](https://huggingface.co/datasets/your-username/vsesvit-dla)
- **PESP corpus**: [apjanco/pesp](https://huggingface.co/datasets/apjanco/pesp)

---

## License

MIT
