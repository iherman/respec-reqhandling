Handling requirements using respec
==================================

Introduction
------------

A small set of functions that can be used together with [respec](https://github.com/w3c/respec) to handle a scenario that is fairly frequent when writing (W3C) specifications:

1.	The group publishes a "Use Cases and Requirements" (a.k.a. UCR) documents. Typically, such a document has a list of requirements; long enough that it may also deserve a separate table and/or list in the document to list those.
2.	Another document is created that has frequent references to the UCR document; more exactly to the requirements listed therein.

The functions in this repo are meant to make these tasks a bit easier.

(Note that `respec` does have some undocumented possibilities for those, but they have proven incomplete for my usage needs. This set is meant to be a replacement for those.)

Usage
-----

See the separate [installation section](#install) on how to install add these functions to the documents.

### The UCR document

See a [toy example](examples/ucr.html).

#### Define requirements

A requirement must be set up as follows.

```
  <p class="req-set" id="myid">My requirement</p>
```

where `p` can be any other element; typical alternatives may be `li` or `span`. In what follows, the string 'My requirement' will be referred to as the [“content”](id:content) of the requirement.

The element is modified by `respec` (and this script) by prepending the content with a

```
  <a href="#myid">Req. i</a>:
```

where the space is a non-breaking space, and `i` is a number starting by 1 and increased for each requirement in document order (regardless of the document’s section numbers). This string will be referred to, in what follows, as the [“title”](id:title) of the requirement.

#### [Storing requirement data in an external file](id:storage)

If the URI of the document is expanded in the browser (e.g., in the address bar) by adding the fragment identifier `#saveReqs`, a dialogue pops up offering the storage of the file in the form of a javascript file. This file should be used in case the requirements are referred to from *another* document.

### [Referring to requirements](id:referring)

This can either be done in the UCR document itself (see [a toy UCR example](examples/ucr.html)), or another document (see [a toy document example](examples/reqrefer.html)).

#### Refer to a requirement

There are three ways to do that. Either:

```
  <a href="#myid" class="req-ref"></a>
```

or

```
  <a href="#myid" class="req-ref-descr"></a>
```

or

```
  <a href="#myid" class="req-ref-full"></a>
```

In the final form reference refers to the requirement itself (i.e., it will be expanded, if applicable, to a full URI; see [setting the UCR's URI](#ucruri) below as part of the installation). The content of the `a` element is the [title](#title) of the requirement, the [content](#content) of the requirement, or the [title](#title) followed by the [content](#content) separated by the ': ' character, respectively. I.e., in our example, the three results are

```
  <a href="URIOUCRDOCUMENT#myid" class="req-ref">Req. i</a>
```

or

```
  <a href="URIOUCRDOCUMENT#myid" class="req-ref-descr">My requirement</a>
```

or

```
  <a href="URIOUCRDOCUMENT#myid" class="req-ref">Req. i: My requirement</a>
```

respectively.

#### Generate a table of requirements

Insert the following table into the document:

```
  <table id="reqtable">
  <!-- possible header rows -->
  </table>
```

or, in a more complex case:

```
  <table id="reqtable">
     <thead>
         <!-- possible header rows -->
     </thead>
     <tbody>
     </tbody>
  </table>
```

The table (or, if applicable, the table body) is expanded by adding table rows, each with two table cells: the [title](#title) and the [content](#content) of the requirement, respectively. The first cell, i.e., the title, is an active link to the requirement itself. There are rows for each requirement, referred to in document order.

#### Generate a list of requirements

Insert the following list into the document:

```
  <ul id="reqlist">
  </ul>
```

(The element `ol` can also be used.) The list is expanded by adding a `li` element, containing the [title](#title) of the requirement as an active link, followed by the [content](#content). A list item is added for each requirement, in document order.

[Installation](id:install)
--------------------------

### [UCR document](id:install_ucr)

1.	Be careful that the reference to `respec` should be synchronous, ie, the attribute `async` should *not* be present
2.	Add into the header (*after* the reference to `respec`\): `<script class="remove" src="scripts/rcollect.js"></script>`.
3.	If the document also uses internal [references to requirements](#referring), add also (after the previous reference): `<script class="remove" src="scripts/rdisplay.js"></script>`.
4.	Add, in `respecConfig`, `preProcess : [rcollect],` or `preProcess : [rcollect, rdisplay],` depending on whether the document uses internal [references to requirements](#referring) or not.
5.	Add, in `respecConfig`, `postProcess: [rstore]` if you intend to store the requirements in a separate file to be used by other documents.

### [Other documents](id:install_other)

1.	Be careful that the reference to `respec` should be synchronous, ie, the attribute `async` should *not* be present
2.	Add, into the header (*after* the reference to `respec`\): `<script class="remove" src="reqInfo.js"></script>`, where `reqInfo.js` is the file generated when [storing the requirements](#storage).
3.	Add, into the header (after the previous reference): `script class="remove" src="scripts/rdisplay.js"></script>`.
4.	Add, in `respecConfig`, `preProcess : [rdisplay],`
5.	[Add](id:ucruri), in `respecConfig`, `ucrUri: "URI-to-the-UCR-Document"` (this entry defaults to the empty string, ie, the fragment identifiers for the requirements will remain intact).

---

30 October 2016
