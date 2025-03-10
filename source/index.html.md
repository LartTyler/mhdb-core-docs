---
title: "Monster Hunter: {{TITLE}} API Reference"

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - shell: cURL

toc_footers:
  - <a href="https://github.com/LartTyler/mhdb-{{ID}}">Visit the project on Github</a>
  - <a href="https://discord.gg/6GEHHQh">Join us on Discord</a>

includes:
  - endpoints_ailments
  - endpoints_armor
  - endpoints_armor_sets
  - endpoints_charms
  - endpoints_decorations
  - endpoints_items
  - endpoints_locations
  - endpoints_monsters
  - endpoints_motion_values
  - endpoints_skills
  - endpoints_weapons
  - data_types
  - searching
  - projecting
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: "Documentation for the unofficial Monster Hunter: {{TITLE}} API"
---

# Introduction
Welcome to the API documentation for [{{URL}}]({{URL}}).

Questions, comments, concerns, complaints? [Join us on Discord!](https://discord.gg/6GEHHQh)

## Accessing the API
The API can be accessed using the base URL `{{URL}}/{locale}`, where `{locale}` is an
[ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1) language code. For example, to access the API in English, you would
use the base URL `{{URL}}/en`.

Note that only the values of certain text fields are localized, not the field names themselves. Additionally, certain
enumerated values (such as [weapon type](#weaponkind)) are not localized.

Some fields may not have localized values for every language. If a field has not been localized for your specified
language, the value will be `null` instead.

### Controlling Response Fields
All endpoints support a `p` query parameter that can be used to control which fields are included in the response
from the API. Read the [Projecting Results](#projecting-results) section for more information.

### Filtering Objects in the Response
All list endpoints support a `q` query parameter that can be used to filter items in the response from the API. A
"list endpoint" is usually a `GET` request to the top-level path hierarchy, e.g. `GET /items`, but endpoints that
support filtering will indicate that they fall into this category. Read the [Searching the API](#searching-the-api)
section for more information.

Such endpoints also support two additional query parameters, `limit` and `offset`, which can be used to paginate
results from the API. The `limit` parameter indicates that maximum number of elements that should be included in the
response, and `offset` is a zero-based index to begin including results from. For example:

`GET {{URL}}/en/items?limit=10&offset=0`

This would return 10 [Items](#items) from the API, beginning at the first element. To move to the second page, you can
simply increase `offset` by the number of elements in each page, like so:

`GET {{URL}}/en/items?limit=10&offset=10`

### Caching
All `GET` requests to the API are cached by Cloudflare, and will include relevant cache headers. This means that while
some requests may be slow for large responses when they are first requested (such as retrieving
[all armor pieces](#list-all-armor)), subsequent requests to the endpoint will be very fast.

Additionally, you can optimize your application further by implementing local caching. Simply include the
[`If-Modified-Since`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since) HTTP header; the
server will respond with a `304 Not Modified` response if your locally cached data is up-to-date.

## Reading this document
All example URLs use the `en` locale (e.g. "{{URL}}/**en**/items"), but your application can use any valid language code.

Some examples have their properties truncated to save space. Any time you see "[...]", this indicates that the example
has been truncated, but there would normally be much more data present in the response.
