---
title: MARC21
---
# MARC21 export

The Libris API for MARC21 export can be accessed at:

https://libris.kb.se/api/marc_export/

To export MARC data, send an HTTP POST request to the above URL with an "export profile" as the message body and four parameters in the URL.

These parameters are:

1. `from` specifies the start of the time interval for which you want updates. The time must be in ISO-8601 format.
1. `until` specifies the end of the time interval for which you want updates. The time must be in ISO-8601 format.
1. `deleted` specifies how you want deletions to be handled. The parameter can have the following values:
    1. `ignore` means that deleted records are simply ignored
    1. `export` means that deleted records are exported but are marked as deleted in the MARC leader
    1. `append` means that the IDs of deleted records are exported as a CSV file _after_ the regular export data (separated by a null byte).
1. `virtualDelete` can be `true` or `false`. If `virtualDelete` is set to `true`, records will be considered deleted in the generated export if the sigels specified in `locations` in the profile no longer have holdings on the records. The flag is preferably used together with `deleted=export`.

## Example request
```bash
$ curl -Ss -XPOST "https://libris.kb.se/api/marc_export/?from=2019-10-05T22:00:00Z&until=2019-10-06T22:00:00Z&deleted=ignore&virtualDelete=false" --data-binary @./etc/export.properties > export.marc
```

## Be careful with time!

BEWARE of your time specifications/time zones! The example above sends times in UTC (hence the 'Z' at the end). This is a good way to do it. If you want to send local times instead of UTC, that's also possible, but then the time zone must be included in the specification. Please read up on ISO-8601!
Make sure the machine calling this API uses NTP, so the machine's clock is correct. If not, you risk missing updates to records. To guard against possible rounding/truncation errors in time specifications, the server also always subtracts one second from the value of `from`. This can be confusing if you try to use the API for statistical purposes or similar.

If you want to call this API with a scheduled script, there are examples/suggestions for such scripts here for:
[Windows](https://github.com/libris/librisxl/blob/master/marc_export/examplescripts/export_windows.bat)
and
[*nix-derivatives (bash)](https://github.com/libris/librisxl/blob/master/marc_export/examplescripts/export_nix.sh).


## Example export profile

```INI
move240to244=off
f003=SE-LIBR
holdupdate=on
lcsh=off
composestrategy=composelatin1
holddelete=off
authtype=interleaved
isbnhyphenate=off
name=EnBibbla
locations=S NB SM SOT SB17 NLT
bibcreate=on
authcreate=on
format=ISO2709
longname=Ett biblbiotek
extrafields=G:050 ; Q:050 ; L:084,650 ; Li:084
biblevel=off
issnhyphenate=off
issndehyphenate=off
holdtype=interleaved
holdcreate=on
characterencoding=UTF-8
isbndehyphenate=off
bibupdate=on
efilter=off
move0359=off
authupdate=on
sab=on
```

Explanation of (some of) the different settings in the export profile:

| parameter            | valid values             | description |
| -------------------- | ----------------------- | ----------- |
| `authcreate`         | `on`\|`off`                | Should newly created authority records result in export.
| `authoperators`      | [list of sigels]          | Space-separated list. Changes to authority records should only result in export if made by one of the following sigels (leave empty for "all").
| `authtype`           | `interleaved`\|`after`     | Determines whether authority information should be embedded in the bib record or follow as a separate record thereafter.
| `authupdate`         | `on`\|`off`                | Determines whether updates to authority records should lead to export.
| `bibcreate`          | `on`\|`off`                | Determines whether creation of a bib record should result in export.
| `biboperators`       | [list of sigels]          | Space-separated list. Changes to bibliographic records should only result in export if made by one of the following sigels (leave empty for "all").
| `bibupdate`          | `on`\|`off`                | Should updates to bibliographic records lead to export.
| `characterencoding`  | `UTF-8`\|`ISO-8859-1`      | Determines character encoding.
| `composestrategy`    | `compose`\|`decompose`     | Determines whether unicode characters should be composed or decomposed.
| `exportdeleted`      | `on`\|`off`                | Should deleted records be exported (but marked as deleted in the MARC leader). Corresponds to the HTTP parameter `deleted=export` or `deleted=ignore`, which takes precedence if both are specified. See also `virtualdelete`.
| `f003`               | [string]                   | Space-separated list. Force field 003 to take a certain value.
| `format`             | `ISO2709`\|`MARCXML`       | Determines serialization format for MARC data.
| `holdcreate`         | `on`\|`off`                | Should newly created holdings lead to export.
| `holddelete`         | `on`\|`off`                | Should deletion of holdings result in export.
| `holdoperators`      | [list of sigels]          | Space-separated list. Changes to holdings records should only result in export if made by one of the following sigels (leave empty for "all").
| `holdtype`           | `interleaved`\|`after`     | Should holdings information be embedded in the bib record or follow as a separate record thereafter.
| `holdupdate`         | `on`\|`off`                | Should updates to holdings lead to export.
| `isbndehyphenate`    | `on`\|`off`                | Remove hyphens in ISBN or not.
| `isbnhyphenate`      | `on`\|`off`                | Add hyphens in ISBN or not.
| `issndehyphenate`    | `on`\|`off`                | Remove hyphens in ISSN.
| `issnhyphenate`      | `on`\|`off`                | Add hyphens in ISSN.
| `lcsh`               | `on`\|`off`                | Generate LCSH fields in the record.
| `locations`          | [list of sigels]\|`*`     | Space-separated list. Generate export for these sigels (or * for all sigels).
| `move0359`           | `on`\|`off`                | Move value from 035$9 to 035$a.
| `move240to244`       | `off`|`on`                 | Move MARC field 240 to 244.
| `nameform`           | `Forskningsbiblioteksform` | Force name forms to take Forskningsbiblioteksform.
| `sab`                | `on`\|`off`                | Should SAB titles (976) be added.
| `virtualdelete`      | `on`\|`off`                | Treat bibliographic records as deleted when the sigels specified in `locations` no longer have any holdings. Preferably used together with `exportdeleted`. Corresponds to the HTTP parameter `virtualDelete`, which takes precedence if both are specified.
| `issnIcFlavour`      | `on`\|`off`                | Change vertical bar ("|") in 008/23 to space.
