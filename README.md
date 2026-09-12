# Memory Palace Library

Ready-made memory palaces for the **Memory Palace** iOS app. The app's Library
tab reads the files in this repository over HTTPS and renders them itself —
this site is never browsed by a person.

Served by GitHub Pages at:

```
https://code-navdeep.github.io/memory-palace-library/
```

Nothing is ever uploaded here by the app. It only reads.

---

## Layout

```
catalog.json                       the index the app fetches
covers/<slug>.jpg                  640×360 cover for each palace
files/<slug>-v<version>.mempal     the palace itself
.nojekyll                          serve every file as-is
```

## `catalog.json`

```jsonc
{
  "version": 1,                       // bump on every catalogue change
  "updatedAt": "2026-09-12T00:00:00Z",
  "palaces": [                        // newest first: the app's default sort
    {
      "slug": "toronto-harbourfront-planets",
      "originID": "8C6C1E90-…",       // the palace's origin UUID — never changes
      "version": 1,                   // bump when you republish this palace
      "name": "Harbourfront Walk",
      "city": "Toronto",
      "country": "CA",                // US or CA
      "text": "The planets in order from the Sun",
      "summary": "One short paragraph shown on the detail screen.",
      "creator": "Navdeep Virdi",
      "difficulty": 1,                // 1 easy … 5 hard
      "stations": 8,
      "walkMetres": 1500,
      "sizeBytes": 4210233,           // exact byte size of the .mempal
      "lookAround": true,             // Look Around at every station?
      "contents": "full",             // "full" or "route"
      "sha256": "…64 hex chars…",
      "cover": "covers/toronto-harbourfront-planets.jpg",
      "file": "files/toronto-harbourfront-planets-v1.mempal"
    }
  ]
}
```

The app refuses a download if it is larger than 25 MB, or if its byte size or
SHA-256 does not match the entry.

## Publishing a palace

1. **Build it** in the app on a device, with Look Around at each station.
2. **Share → Full Palace** (or Route Only) and save the `.mempal` to Files, then
   move it to your Mac.
3. **Rename** it `<slug>-v<version>.mempal` and put it in `files/`.
4. **Cover**: export a 640×360 JPEG (a screenshot of the palace's entrance
   station, or its composited thumbnail) to `covers/<slug>.jpg`.
5. **Measure** the file:
   ```sh
   wc -c < files/<slug>-v1.mempal      # sizeBytes
   shasum -a 256 files/<slug>-v1.mempal # sha256
   ```
6. **Add the entry** to the top of `catalog.json`'s `palaces` array, bump the
   top-level `version` and `updatedAt`.
7. **Commit and push** (or upload through the GitHub web interface). Pages
   republishes in about a minute; the app shows it on the next refresh.

### Updating a palace already in the library

Keep the same `slug` and `originID`, raise `version`, add the new file as
`<slug>-v<version>.mempal`, and point `file` at it. The app shows **Update** to
anyone who installed the older version; choosing it replaces their copy.

## Credit and licence

Every entry carries a `creator`; the app shows
"Shared by <creator> · free for personal use".

Palaces are shared for personal study. Texts used in them are public domain or
plain fact. Do not publish a palace containing copyrighted text, or one whose
stations sit on private property.
