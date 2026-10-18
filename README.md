# Lab 4 — XML and JSON (Web Development)

**Student ID:** 247052

**Live site (GitHub Pages):**
### https://wanggenji404.github.io/247052-lab4/

| Page | URL | Task |
|---|---|---|
| Book Catalogue (XML) | https://wanggenji404.github.io/247052-lab4/index.html | Task 1 — XML Lab |
| User Profiles (JSON) | https://wanggenji404.github.io/247052-lab4/users.html | Task 2 — JSON Lab |

**Repository:** https://github.com/wanggenji404/247052-lab4 (public)

---

## Files

| File | Purpose |
|---|---|
| `book.xml` | Task 1 data — the book catalogue (6 books) |
| `userProfiles.json` | Task 2 data — 4 user profiles |
| `index.html` | Task 1 page — fetches `book.xml` and renders the books |
| `users.html` | Task 2 page — fetches `userProfiles.json` and renders the profiles |
| `styles.css` | Shared styling for both pages |
| `screenshots/` | Evidence of the working site |

---

## Task 1 — XML Lab

### `book.xml`

A well-formed XML document with a `bookCatalogue` root and six `book` elements.
Each book carries an **`id` attribute** and three child elements —
**`title`**, **`author`** and **`price`**:

```xml
<book id="B001">
  <title>The Pragmatic Programmer</title>
  <author>Andrew Hunt and David Thomas</author>
  <price>79.99</price>
</book>
```

### Validation

The document was checked in three ways:

1. **Well-formedness** — it parses as XML without errors:

   ```
   [xml] cast                    -> OK, root element = bookCatalogue, 6 <book> elements
   DOMParser in the browser      -> no <parsererror> element
   ```

2. **Structure** — every `book` has an `id` attribute and exactly the three
   required children; verified by walking the parsed document:

   ```
   B001 id=B001  title=The Pragmatic Programmer   author=Andrew Hunt and David Thomas  price=79.99
   B002 id=B002  title=Clean Code                 author=Robert C. Martin             price=65.50
   B003 id=B003  title=Design Patterns...         author=Erich Gamma et al.           price=89.00
   B004 id=B004  title=Refactoring...             author=Martin Fowler                price=72.25
   B005 id=B005  title=JavaScript: The Definitive Guide  author=David Flanagan        price=59.95
   B006 id=B006  title=Learning XML               author=Erik T. Ray                  price=45.00
   ```

3. **Online validator** — the file also loads without error in
   <https://www.xmlvalidation.com> (paste the contents, "Validate XML").

### `index.html`

Fetches the XML, parses it with `DOMParser`, and renders one card per book.

```js
fetch('book.xml')
  .then(response => {
    if (!response.ok)
      throw new Error('Fetch error: ' + response.status)
    return response.text();
  })
  .then(xmlText => {
    const parser = new DOMParser();
    const xmlDoc = parser.parseFromString(xmlText,'text/xml');
    // DOMParser does not throw on bad XML - it returns a <parsererror> document
    if (xmlDoc.getElementsByTagName('parsererror')[0]) throw new Error(...);
    ...
  })
  .catch(err => console.error('Fetch error: ' + err.message));
```

What the page shows:

- a status line — *Loaded 6 books from book.xml*
- a summary strip — number of books, total catalogue value, average price
- one card per book: `id` badge, title, author, price formatted as `$79.99`
- a readable error box plus a `console.error` if the fetch or the parse fails

---

## Task 2 — JSON Lab

### `userProfiles.json`

Four profiles. Each has **`firstName`**, **`lastName`**, **`email`**,
**`dateOfBirth`**, an **`address`** object and a **`preferences`** object:

```json
{
  "firstName": "Aroha",
  "lastName": "Ngata",
  "email": "aroha.ngata@example.com",
  "dateOfBirth": "1994-03-17",
  "address": {
    "street": "12 Fitzherbert Avenue",
    "city": "Palmerston North",
    "state": "Manawatu-Whanganui",
    "zipCode": "4410"
  },
  "preferences": {
    "theme": "dark",
    "language": "en-NZ",
    "notifications": true,
    "subscription": true
  }
}
```

Both parts of the *extend the JSON* step are present in every profile:

- **`address`** — added to the profile, with `street`, `city`, `state`, `zipCode`
- **`subscription`** — added to `preferences`, a boolean (`true` / `false`)

### Validation

- Parses as JSON without errors (`JSON.parse` / `ConvertFrom-Json` both succeed).
- Every profile has all six top-level fields; all four address fields and all
  four preference fields are present.
- Also validated by pasting into <https://jsonformatter.currencies.io> /
  <https://jsonformatter.org> — no errors reported.

### `users.html`

Fetches the JSON and renders the profiles **as cards and as a table**:

```js
fetch('userProfiles.json')
  .then(response => {
    if (!response.ok)
      throw new Error('Fetch error: ' + response.status)
    else
      return response.json();
  })
  .then(json => { /* render the profiles */ })
  .catch(err => console.error('Fetch error: ' + err.message));
```

What the page shows:

- a status line — *Loaded 4 user profiles from userProfiles.json*
- a summary strip — number of profiles, how many subscribed, how many with
  notifications on
- one card per profile with three labelled sections:
  - **Details** — first name, last name, email, date of birth
    (shown as *17 March 1994* rather than `1994-03-17`)
  - **Address** — street, city, state, zip code
  - **Preferences** — theme, language, notifications and newsletter
    subscription as coloured on/off pills
- a summary table underneath with one row per profile, so every field is
  visible at a glance

---

## Running it locally

Opening the HTML files directly from disk (`file://`) does **not** work: the
browser blocks `fetch()` for local files. Serve the folder over HTTP instead:

```bash
# any static server works, for example
python -m http.server 8000
# then open http://localhost:8000/index.html
```

The site is deployed to GitHub Pages, so it works with no local setup at all.

---

## How GitHub Pages was set up

1. Created the public repository `247052-lab4`.
2. Pushed the six site files to `main`.
3. Repository **Settings → Pages → Build and deployment**:
   source = *Deploy from a branch*, branch = **main**, folder = **/ (root)**.
4. GitHub published the site at https://wanggenji404.github.io/247052-lab4/.

A `.nojekyll` file is included so GitHub serves the files as-is instead of
running them through Jekyll.

---

## Screenshots

| File | Shows |
|---|---|
| `screenshots/01-books-page.png` | The live XML page with all six books rendered |
| `screenshots/02-users-page.png` | The live JSON page with the profile cards and the table |
| `screenshots/03-book-xml.png` | The `book.xml` file source |
| `screenshots/04-user-profiles-json.png` | The `userProfiles.json` file source |

All page screenshots were taken from the **deployed** GitHub Pages URLs, so they
show the site as the marker will see it.
