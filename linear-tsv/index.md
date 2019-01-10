---
title: Zegami TSV Format
subtitle: simple, line-oriented, tabular data
author: Jason Dusek <jason.dusek@gmail.com>
layout: spec
listed: true
version: 1.0-beta
updated: 10 January 2019
created: 6 May 2014
summary: This document defines a format for tabular data.

---

Summary
=======

This document defines a line-oriented, tabular data format similar to that
produced by relational databases.

### Changelog

- `1.0-beta`: initial revision
- `zegami`: Zegami edits


## Specification

A tabular data file consists of zero or more *records* consisting of *fields*.
Encoding should be utf-8.

Records are separated by ASCII newlines (`0x0a`) (unix line endings).
Fields within a record are separated with ASCII tab (`0x09`).
It is permitted but discouraged to separate
records with carriage-return-newline (`0x0d` and `0x0a`). (A literal carriage
return in any other position is non-conforming.)

Zegami expects there to be a header line across the top of the file. This
should give names to each of the columns and should not contain empty values.
Field names should not be repeated.

All tabs, carriage returns and newlines in text fields must be removed and
replaced with spaces to allow Zegami to process the data.

Records must contain at least one field. All fields must be present in every
record. To indicate missing data for a field, the field is left blank.
or the character sequence `\N`
(bytes `0x5c` and `0x4e`) is used. Note that the `N` is capitalized. This
character sequence is exactly that used by SQL databases to indicate SQL
`NULL` in their tab-separated output mode.

If a single backslash is encountered at the end of a field, it is an error. If
a backslash precedes another character but does not form a null sequence `\N`,
it is a "superfluous backslash" and is removed from the field
on read. Such a "superfluous backslash" must never be written by a conforming
implementation.

