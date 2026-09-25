# Counterfactual synthetic CT scans — interactive demo

A static, in-browser viewer for a curated subset of anatomically-constrained counterfactual
synthetic CT scans: pulmonary embolism (CT pulmonary angiography) and intracranial hemorrhage
(non-contrast head CT). For each disease, 3 source patients are shown, each with 3
counterfactuals (the same synthetic patient with a different intended lesion) and the
patient's synthetic no-lesion control. **All scans are synthetic.**

Each view shows axial, coronal and sagittal planes with the intended-lesion mask, adjustable
window/level, a magnifier, the conditioning impression, whether each detector found the
lesion, and a radiologist's rating and description.

## Viewing

- **GitHub Pages:** *Settings → Pages → Build and deployment → Deploy from a branch*, select
  the branch and `/ (root)`. No build step is needed.
- **Locally:** run `python -m http.server 8000` in this folder and open
  <http://localhost:8000>. Opening `index.html` directly from disk does not work, because
  browsers block `fetch()` from `file://` pages.
- **Deep links:** append a scan id, e.g. `index.html#pe1_cf1` or `index.html#ich2_ctl`.

## Data format

- `data/manifest.json` — scan list, display metadata, and the 8-bit code → HU table per disease.
- `data/<id>/v<k>.webp` — the CT volume as 8-bit axial slices, 4×4 slices per sheet. The code
  is piecewise-linear in HU (fine steps inside the default display window), so window/level
  works in the browser. ICH volumes keep every second axial slice (0.28 mm → 0.56 mm).
- `data/<id>/m<k>.png` — the intended-lesion mask (1-bit), cropped to its bounding box.

Everything is rendered client-side; there is no server component.
