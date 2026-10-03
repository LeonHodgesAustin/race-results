# Vendored map and chart libraries

Pinned copies of the two libraries every dashboard page uses, so a page can
load them from beside itself instead of a CDN. `dashboard.install_vendor_assets()`
copies this whole folder next to rendered pages (`site/vendor/` for `publish`,
`reports/vendor/` for `report`/`dashboard`), and `render_dashboard_html(...,
vendor_base=...)` makes each page try that copy first, then the CDN.

| File | Version | Source | sha256 | License |
|---|---|---|---|---|
| `leaflet-1.9.3/leaflet.js` | Leaflet 1.9.3 | npm `leaflet@1.9.3/dist/` (jsDelivr; matches unpkg) | `5819285cec137b229c94e1ee5ad73e8b6b84345a4367d60f75fe477fe0fb7b03` | BSD-2-Clause (`leaflet-1.9.3/LICENSE`) |
| `leaflet-1.9.3/leaflet.css` + `images/` | Leaflet 1.9.3 | same | css `90b693d86392a4779c861b28cf307e7e59c3fb35328c4d8b95f58f814d38c722` | same |
| `plotly-basic-2.35.2.min.js` | plotly.js 2.35.2, **basic** build | `cdn.plot.ly/plotly-basic-2.35.2.min.js` (matches npm `plotly.js-basic-dist-min@2.35.2`) | `138c2e81014b979dc00867a93da55b7605a17495ee78dd7afb433b7f021dfcfa` | MIT (`plotly-basic-2.35.2.LICENSE`) |

The basic build (scatter, bar, pie; ~1 MB instead of the full bundle's
4.6 MB) is enough: every chart on the dashboard is a scatter trace. If a
future chart needs another trace type, switch to a build that has it.

**Upgrading:** drop the new files in under new version-named paths, delete
the old ones, and bump `_LEAFLET_VERSION` / `_PLOTLY_VERSION` in
`analysis/dashboard.py` (the render fingerprint includes them, so `publish`
re-renders every page). Keep the names version-specific: the GitHub Pages
copy is merged, never mirrored, so a page built by an older version keeps
finding the files it was built against.
