---
title: SRU
---
--8<-- "docs/snippets/_user_agent.sv.md"

Bas-URL: https://libris.kb.se/sru

## Exempel

```bash title="Shell"
curl -XGET 'https://libris.kb.se/sru/libris?operation=searchRetrieve&version=1.2&query=%22skatt%22+sortBy+libris.legacysort'
```
