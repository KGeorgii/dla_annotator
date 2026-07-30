# DLA Annotator

A single-file, browser-based bounding-box annotation tool for document layout analysis (DLA) of digitized periodical pages. No install, no server, no build step — it runs at **[kgeorgii.github.io/dla_annotator](https://kgeorgii.github.io/dla_annotator/)**.

Built to produce the training data for [vsesvit-layout-yolov8m](https://huggingface.co/kgeorgii/vsesvit-layout-yolov8m), a YOLOv8m layout detection model for Ukrainian Soviet-era periodicals. The released weights predict classes 0–7. The annotator has since moved to an expanded 11-class taxonomy, so it currently serves as the labeling tool for an updated model — exports made today are training data for the next version, not labels the released weights can reproduce.

---

## Features

- **Drag-and-drop PDF loading** — load one or multiple PDFs at once; all pages are listed in the left panel
- **11-class label taxonomy** — `journal_name`, `article_title`, `author_name`, `page_number`, `text_block`, `image`, `mixed_text`, `decorative_element`, `cover`, `image_caption`, `artist_photographer`
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

1. Open **[kgeorgii.github.io/dla_annotator](https://kgeorgii.github.io/dla_annotator/)**
2. Drag PDF files onto the window, or click **Load PDF(s)**
3. Select a label class (right panel or keys `1`–`9`, `0`, `Q`)
4. Draw boxes by clicking and dragging on the page
5. Fill in the metadata fields (journal, year, issue, source URL)
6. Click **Save Page** or press `S` before navigating away
7. When done, click **Export ↓** to download your annotations

Nothing is uploaded — PDFs are parsed in the browser and annotations are held in the page's memory. **Save Page** commits the current page to that in-memory store, not to disk, so export before closing the tab. Use **Import ↑** with a previously exported JSONL to resume a session.

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `1` – `9`, `0`, `Q` | Select label class |
| `←` / `→` | Previous / next page |
| `Del` / `Backspace` | Delete selected annotation |
| `Esc` | Deselect annotation |
| `+` / `-` | Zoom in / out |
| `S` | Save current page |

Shortcuts are derived from the `CLASSES` array, so a new class only needs a `key` field — as long as it avoids `s`, `+`, `-`, and `=`.

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

| ID | Class | |
|----|-------|--|
| 0 | `journal_name` | |
| 1 | `article_title` | |
| 2 | `author_name` | |
| 3 | `page_number` | |
| 4 | `text_block` | |
| 5 | `image` | |
| 6 | `mixed_text` | |
| 7 | `decorative_element` | |
| 8 | `cover` | new |
| 9 | `image_caption` | new |
| 10 | `artist_photographer` | new |

IDs 0–7 are frozen: the three new classes were appended rather than slotted in, so label files and JSONL exports made under the original taxonomy remain valid without remapping. Training on the full taxonomy needs `nc: 11` in `data.yaml` and a fresh detection head — the released 8-class weights can't be fine-tuned onto it directly.

---

## Dependencies

- [PDF.js 3.11.174](https://mozilla.github.io/pdf.js/) — loaded from the Cloudflare CDN, no local install needed
- Everything else is vanilla HTML, CSS, and JavaScript

---

## Related

- **Model**: [kgeorgii/vsesvit-layout-yolov8m](https://huggingface.co/kgeorgii/vsesvit-layout-yolov8m)
- **Dataset**: [kgeorgii/vsesvit-dla](https://huggingface.co/datasets/kgeorgii/vsesvit-dla)
- **PESP corpus**: [apjanco/pesp](https://huggingface.co/datasets/apjanco/pesp)

---

## License

MIT
