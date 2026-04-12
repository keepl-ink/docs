# Documentation


## Interfaces
### SavedUrl
```ts
interface SavedUrl {
	url: string;
	title: string
	savedAt: number;
}
```


## API Contract
All API routes are prefixed by the configured `Backend URL`

### `GET /healthz`
This route gets called when a new backend url gets sets, so that the validity/existance of the server can be confirmed.

Body is ignored. Any non-2xx response is treated as a configuration error and is shown as error to the user.


---

### `GET /urls`
Called on every given interval on initial sync.

#### Response Body
```ts
{
  count: number,
  items: SavedUrl[]
}
```

---

### `POST /urls`
Called every time a page is saved. Use this to index the page on the backend (e.g. fetch content, generate embeddings, persist to a DB).

#### Request body

```ts
{
  url:     string;  // full URL        — "https://example.com/article"
  title:    string;  // page <title>    — "My Article | Example"
  savedAt: number;  // Unix ms         — 1711234567890
}
```

#### Response `200 OK`

Body should be ignored by the extension.


---

### `DELETE /urls/:url`

Called every time a saved page is removed. `:url` is the full URL, **url-encoded**.


#### Example

```
DELETE /urls/https%3A%2F%2Fexample.com%2Farticle
```

#### Response `200 OK` or `204 No Content`

Body is ignored.


---




