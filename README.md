# Tsuki Parser Templates

Community-maintained parser templates for the [Tsuki Manga Reader](https://github.com/Space4414/Tsuki) Android app.

The app checks this repository at startup and automatically downloads any template you don't have locally, so new site support and parser fixes reach users **without an APK update**.

---

## How it works

1. On startup (and every 24 hours), Tsuki fetches [`index.json`](index.json) from this repository.
2. For each entry in the index that is newer than the locally-installed version, the app downloads the template JSON file.
3. The template is saved locally. Users can then tap **Add Site** on any template to point it at a compatible manga website.

---

## Template JSON format

Each template is a `.json` file in `templates/`. Required top-level fields:

| Field | Type | Description |
|---|---|---|
| `name` | string | Unique identifier shown in the app (e.g. `"Madara"`) |
| `version` | string | Semver string — bump this when you update a template (e.g. `"1.2"`) |
| `type` | string | `"html"` for HTML scrapers, `"json"` for REST API scrapers |
| `mangaList` | object | How to fetch the series listing |
| `pageList` | object | How to extract page image URLs from a reader page |

Optional sections: `mangaDetail`, `chapterList`, `genres`.

### `mangaList` fields

| Field | Default | Description |
|---|---|---|
| `endpoint` | `/` | Path (or absolute URL) for the listing page |
| `method` | `GET` | `GET` or `POST` |
| `action` | — | WordPress AJAX action name (POST only) |
| `pagination` | `page` | `page` (1-based), `offset`, or `ajax` |
| `pageParam` | `page` | Query-string key for the page number |
| `itemSelector` | auto-detect | CSS selector for each manga card |
| `titleSelector` | auto-detect | CSS selector for the title within a card |
| `coverSelector` | auto-detect | CSS selector for the cover `<img>` |
| `linkSelector` | auto-detect | CSS selector for the detail-page `<a>` |
| `searchEndpoint` | `/` | Path for search results |
| `searchParam` | `s` | Query-string key for the search query |

### `mangaDetail` fields

| Field | Default | Description |
|---|---|---|
| `titleSelector` | `h1` | CSS selector for the manga title |
| `coverSelector` | `img` | CSS selector for the cover image |
| `descriptionSelector` | — | CSS selector for the synopsis/description |

### `chapterList` fields

| Field | Default | Description |
|---|---|---|
| `selector` | built-in cascade | CSS selector for each chapter row |
| `titleSelector` | `a` | CSS selector for the chapter title within a row |
| `linkSelector` | `a` | CSS selector for the chapter link |
| `action` | — | WordPress AJAX action for chapter lists loaded via AJAX |
| `endpoint` | — | AJAX endpoint path (usually `/wp-admin/admin-ajax.php`) |

### `pageList` fields

| Field | Default | Description |
|---|---|---|
| `imageSelector` | `img` | CSS selector for reader page `<img>` elements |

### `genres` fields

| Field | Required | Description |
|---|---|---|
| `endpoint` | yes | Path to the page that lists all genres |
| `selector` | yes | CSS selector for each genre link |

---

## Contributing a new template

1. Fork this repository.
2. Copy one of the existing templates as a starting point.
3. Edit the selectors to match the target site.
4. Add your template to `index.json` with a unique `name`, `version: "1.0"`, and the `file` path.
5. Open a pull request — maintainers will review and merge.

### Testing your template

Before submitting, test your template by:
1. Exporting your working template from Tsuki (Parsers → your template → Export).
2. Verifying it can list manga, show details, list chapters, and open pages on at least 2-3 compatible sites.

---

## Template index (`index.json`)

The index is a JSON file at the root of this repository:

```json
{
  "schemaVersion": 1,
  "updatedAt": "2026-05-14",
  "templates": [
    {
      "name": "Madara",
      "version": "1.3",
      "file": "templates/madara.json",
      "description": "WordPress Madara theme"
    }
  ]
}
```

Bump `version` whenever you update a template file — the app uses this to decide whether to download the updated file.

---

## License

Template JSON files are released under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) — do whatever you want with them.
