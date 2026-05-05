---
title: Setting a User-Agent header
---

When making requests to any Libris API, please include a descriptive [`User-Agent` header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/User-Agent). This makes it possible for us to identify your requests in server logs and contact you if necessary.

Without a meaningful `User-Agent`, your requests appear as anonymous traffic. If your use of Libris APIs causes unexpected load or errors we have no way to notify you. In cases of repeated problems, anonymous clients may be rate-limited or blocked without warning.

## What to include

A good `User-Agent` string identifies your application and provides a way to contact you:

```
User-Agent: my-library-app/1.0 (https://example.com/; contact@example.com)
```

At minimum, include:

- The name of your application or script and a version number
- A URL or email address so we can contact you if needed.

## Examples

### curl

```bash
curl -H "User-Agent: my-library-app/1.0 (contact@example.com)" \
     https://libris.kb.se/find.jsonld?q=...
```

### Python (requests)

```python
import requests

headers = {"User-Agent": "my-library-app/1.0 (contact@example.com)"}
response = requests.get("https://libris.kb.se/find.jsonld", params={"q": "..."}, headers=headers)
```

### JavaScript (fetch)

```js
fetch("https://libris.kb.se/find.jsonld?q=...", {
  headers: { "User-Agent": "my-library-app/1.0 (contact@example.com)" },
});
```
