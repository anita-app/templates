# Anita templates

Ready-made project structures offered by [Anita](https://anita-app.com) when a user creates a project.

The app downloads [`templates.json`](templates.json) from this repository every time the template gallery is opened
(it keeps the last copy for offline use), so **changes merged into `main` are visible to users within minutes**,
without releasing the app.

## Editing

- Edit `templates.json` and open a pull request. Editors with JSON Schema support validate it against
  [`templates.schema.json`](templates.schema.json).
- `samples` holds a few realistic example records per section, which users can choose to start with (see below).
- Keep `id`s of templates, categories and section `key`s stable.
- `fieldName` is the key under which values are saved: lower camelCase, unique within a section, never `id`,
  `createdAt`, `updatedAt`, `parentsInfo`, `createdBy`.
- The first field of a section is used as the title of its records: make it a short text, usually required.
- Icons are [Ionicons](https://ionic.io/ionicons) names in camelCase with the `Outline` suffix, e.g. `bookOutline`.
- `childOf` lists the keys of parent sections, which must come earlier in the same template. Records of the child
  section can then be linked to records of the parent (e.g. Tasks of a Project).

## Example records (`samples`)

When creating a project from a template, users can add some example records to see how it works. Keep them few
(two or three per section), realistic and neutral: a brownies recipe, a Michael Crichton novel, `example.com` emails.

```json
"samples": {
  "authors": [{ "ref": "crichton", "values": { "name": "Michael Crichton" } }],
  "books": [{ "values": { "title": "Jurassic Park", "status": "read", "finishedOn": "@today-40" }, "parents": ["authors:crichton"] }]
}
```

- `values` are keyed by `fieldName`. Drop-down lists and alternative choices take the option `value`; check boxes
  `true`/`false`; numbers are numbers.
- Dates are `YYYY-MM-DD`, or relative to the day the project is created: `@today`, `@today+7`, `@today-3`. They are
  adapted to the field type (date, month, date & time).
- Long texts are plain text, one paragraph per line.
- `ref` names a record so that records of child sections can link to it with `parents: ["sectionKey:ref"]`. The
  section must be in the child's `childOf`.

## Field types (`componentCode`)

| code | field | `type` | `options` |
| --- | --- | --- | --- |
| 1 | Short text | `text`, `email`, `number`, `tel`, `url`, `color`, `range`, `password` | |
| 2 | Long text with formatting | | |
| 3 | Check box | | |
| 4 | Drop-down list | | `[{ "value": "done", "label": "Done" }]` |
| 5 | Alternative choice | | as above |
| 8 | Date | `date`, `month`, `week` | |
| 9 | Date & time | `datetime-local`, `time` | |
| 12 | Long plain text | | |

## Example

```json
{
  "id": "reading-list",
  "title": "Reading list",
  "description": "Books you want to read, are reading and have read.",
  "category": "learning",
  "tags": ["books"],
  "icon": "bookOutline",
  "sections": [
    {
      "key": "books",
      "title": "Books",
      "icon": "bookOutline",
      "fields": [
        { "label": "Title", "fieldName": "title", "componentCode": 1, "type": "text", "required": true },
        { "label": "Status", "fieldName": "status", "componentCode": 4, "options": [
          { "value": "toRead", "label": "To read" }, { "value": "read", "label": "Read" }
        ] }
      ]
    }
  ]
}
```

## License

CC0: use these templates for anything.
