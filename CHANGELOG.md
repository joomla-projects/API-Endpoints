# Changelog

Each release describes the core Web Services API of one Joomla minor version: release 6.2.x is for
Joomla 6.2. A patch release (6.2.1, 6.2.2, ...) improves the description of the same API.

## Unreleased

Request-body defaults now say what the webservices do with an omitted field (checked on a Joomla 6.2 site and
against the install SQL of joomla-cms `6.2-dev`):

- On create, the webservices do not apply the defaults of the administrator forms. An omitted field gets the
  default of its database column:
  - `state` or `published` is 0 (unpublished), not 1, for articles, banners, banner clients, categories,
    contacts, fields, field groups, content languages, menu items, modules, newsfeeds and tags;
  - `track_impressions` and `track_clicks` of banners and banner clients are -1 (use the client default or the
    global setting), not 0;
  - `numarticles` of newsfeeds is 1, not 5.
- `published` of a redirect is required: its column has no default, so saving fails without it.
- On update (PATCH), an omitted field keeps its stored value. The `default` of the 131 top-level properties of
  PATCH request bodies is removed, so that clients do not send it and overwrite the stored value. The form
  default stays in `x-joomla-form.default`.
- `list[ordering]` and `list[direction]` of the article list are described: the accepted columns, the ones that
  give an internal server error, and the defaults (by id, descending).

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
