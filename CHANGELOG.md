# Changelog

Each release describes the core Web Services API of one Joomla minor version: release 6.2.x is for
Joomla 6.2. A patch release (6.2.1, 6.2.2, ...) improves the description of the same API.

## Unreleased

The `required` lists and minimal examples of POST request bodies now match what Joomla needs to save a new
item (each minimal example was sent to a Joomla 6.2 site; the causes were checked in joomla-cms `6.2-dev`). A
field whose database column has no default is required: without it, saving fails with "Field 'x' doesn't have
a default value".

- Banners: `description`, `custombannercode` and `params` are required. The minimal example used `catid` 1,
  which is not a banner category: Joomla then creates a new banner category titled "1". It now uses 3.
- Content languages: `image`, `description` and `metadesc` are required. The minimal example had an invalid
  `lang_code`.
- Menu items (site and administrator): `alias` is required, an empty string is accepted. Without it, saving fails
  with "Field 'path' doesn't have a default value", as the path is copied from the alias before Joomla makes the
  alias from the title. `link` is required for administrator menu items as well. `type` lists the accepted
  types: an unknown type, as in the old minimal example, gives an internal server error.
- Articles: `introtext` or `articletext` is required.
- Required JSON objects (`params` of banners, fields, modules and template styles; `images`, `metadata` and
  `params` of newsfeeds) need at least one property that the form knows (`minProperties: 1`): the form filter
  drops an empty object, and saving fails.
- Users: `groups` needs at least one group (`minItems: 1`).
- Administrator menus and modules: `client_id` is not set by the route, as was described. Without it, Joomla saves
  a site menu or site module.
- Media files: the `mediatypes` query parameter is described. Without it, only image extensions are accepted, so
  the minimal example now uploads a PNG instead of a `.txt` file.
- Redirects: the minimal example had the same old and new URL, which Joomla rejects.
- Messages: POST saves the message, but Joomla 6.2 answers 404.

## 6.2.2

Response attributes now match what the API views of Joomla render (checked against joomla-cms 6.2.0 and
`6.3-dev`, whose API code is the same):

- `id` added to the attributes of 104 responses (GET, POST and PATCH on 64 paths). It was missing in 68
  responses with `additionalProperties: false`, so a real response did not validate against the spec.
- `created_by`, `modified_by`, `tags` and (contacts) `user_id` added as attributes of articles, contacts and
  newsfeeds. Joomla renders them as attributes as well as relationships.
- `urls` removed from the attributes of the article list: the list query does not select it.
- `articletext`: the request bodies of articles say that it has no effect on update, and that `introtext` and
  `fulltext` win over it on create.
- The `tags` of contacts and newsfeeds describe a bug in Joomla: with more than one tag, a title can be paired
  with the wrong tag ID (the same bug was fixed for articles in Joomla 4.4.0).

## 6.2.1

The specification is the same as in 6.2.0; only `info.version` changed.

- New release file `joomla-ws-api-6.2.1-phpstorm.zip`: a collection for the HTTP Client of PhpStorm and other
  JetBrains IDEs, with a request for each operation, generated from the specification.
- The [`phpstorm`](https://github.com/joomla-projects/API-Endpoints/tree/phpstorm) branch has the collection of the
  newest specification in `main`.

## 6.2.0

First release in this repository: the specification that is published in the
[Joomla Manual](https://manual.joomla.org/docs/next/web-services-api/openapi/) as `joomla-6.2.0.json`
([joomla/Manual#724](https://github.com/joomla/Manual/pull/724)).

- 235 operations on 110 paths: all routes of the core web services plugins of Joomla 6.2.0.
- The title is now "Joomla 6.2 Core Web Services API", without the patch version.
- The responses of each operation are in numerical order (200, 204, 400, ..., 500).
