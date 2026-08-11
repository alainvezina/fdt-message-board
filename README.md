# Fil Live — `live-ops.json` schema

Source of messages for the Festival à deux têtes mobile companion (`/live`).

| | |
|--|--|
| **File** | `live-ops.json` |
| **Repo** | https://github.com/alainvezina/fdt-message-board |
| **Raw URL (app fetch)** | `https://raw.githubusercontent.com/alainvezina/fdt-message-board/master/live-ops.json` |
| **Edit** | Change the file on GitHub → save. No FDT deploy. |
| **Refresh** | App re-fetches about every 60s and when the tab becomes visible. |

---

## Root shape

Either:

1. A **JSON array** of message objects (recommended), or  
2. An object `{ "messages": [ ... ] }`

Invalid rows are ignored. On fetch failure, the fil shows empty: *« Aucune info Live pour le moment. »*

---

## Message object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | yes | Stable unique id (e.g. `ops-welcome`, `ops-delay-eglise-2130`). |
| `text` | string | yes | Message shown in the fil (French, short, one or two lines). |
| `publishedAt` | string (ISO 8601) | yes | Publish time. App shows « Il y a X min » from this. Use America/Toronto offset in summer: `-04:00`. |
| `kind` | string | no | Icon type. See table below. Default / unknown → info icon. |
| `urgent` | boolean | no | If `true`, urgent styling and sorts above non-urgent. Default `false`. |
| `href` | string | no | Optional link. Relative (`/info#navette`, `#carte`) or absolute `https://...`. |

Do **not** put `minutesAgo` in the file. Age is computed in the app.

---

## `kind` values

| Value | Use |
|-------|-----|
| `info` | General / welcome |
| `delay` | Show delayed or schedule change |
| `navette` | Shuttle |
| `parking` | Parking |
| `water` | Drinking water |
| `toilets` | Toilets |
| `crowd` | Crowd / queue |
| `lost` | Lost and found |
| `weather` | Weather / clothing |
| `food` | Food / drink kiosks |

---

## Sort order (app)

1. `urgent: true` first  
2. Then newest `publishedAt` first  

Array order in the file does not matter after fetch.

---

## Minimal example (open day)

```json
[
  {
    "id": "ops-welcome",
    "text": "Bienvenue au Festival à deux têtes. Prenez soin de vous et des autres, et bon festival!",
    "kind": "info",
    "publishedAt": "2026-08-12T10:00:00-04:00"
  }
]
```

## Ops update example

```json
[
  {
    "id": "ops-delay-eglise-2130",
    "text": "Scène Desjardins - Église : début reporté de 10 min",
    "kind": "delay",
    "urgent": true,
    "publishedAt": "2026-08-13T21:30:00-04:00"
  },
  {
    "id": "ops-welcome",
    "text": "Bienvenue au Festival à deux têtes. Prenez soin de vous et des autres, et bon festival!",
    "kind": "info",
    "publishedAt": "2026-08-12T10:00:00-04:00"
  }
]
```

---

## Ops checklist

1. Prefer short, true, useful lines (avoid fake delays or fake queues).  
2. New alert → new `id` + current `publishedAt` + set `urgent` if needed.  
3. Stale items → delete or edit in GitHub so the fil stays short (about 3–8 items).  
4. After save, wait up to ~1 minute or reopen `/live` on the phone.  
5. Use the **raw** URL only for debugging; the blob GitHub page is HTML, not the feed.

---

## When the companion is open (public)

Automatic window (device clock):

- **Opens:** 2026-08-12 10:00 America/Toronto  
- **Closes:** 2026-08-16 23:59:59 America/Toronto  

Outside that window: closed page, unless `?demo=1`.
