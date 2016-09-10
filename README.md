# Handling requirements using respec

Just a small set of functions that can be used together with [respec](https://github.com/w3c/respec) to handle a scenario that is fairly frequent when writing (W3C) specifications:

1. The group publishes a  "Use Cases and Requirements" (a.k.a. UCR) documents. Typically, such a document has a list of requirements; long enough that it may also deserve a separate table and/or list in the document to list those.
2. Another document is created that has frequent references to the UCR documents; more exactly to the requirements listed therein.

The functions in this repo are meant to make these tasks a bit easier.

(Note that respec does have some undocumented possibilities for those, but they have proven way too poor for my usage. This set is meant to be a replacement for those.)

