---
title: SRU
---
--8<-- "docs/snippets/_user_agent.en.md"

Base URL: https://libris.kb.se/sru

## Example

```bash title="Shell"
curl -XGET 'https://libris.kb.se/sru/libris?operation=searchRetrieve&version=1.2&query=%22skatt%22+sortBy+libris.legacysort'
``` 