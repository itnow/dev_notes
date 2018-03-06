===============================================================================
lxml.html
===============================================================================

http://lxml.de/lxmlhtml.html

**lxml** comes with a dedicated Python package for dealing with HTML:
**lxml.html**. It is based on lxml's HTML parser, but provides a special
Element API for HTML elements, as well as a number of utilities for common HTML
processing tasks.

The main API is based on the lxml.etree API, and thus, on the ElementTree API.


lxml.etree
----------

An implementation of the ElementTree API in lxml, a library which is built on
top of libxml2. lxml is really fast and has many extensions exposing advanced
libxml2/libxslt features.

The lxml XML toolkit is a Pythonic binding for the C libraries libxml2 and
libxslt. It is unique in that it combines the speed and XML feature
completeness of these libraries with the simplicity of a native Python API,
mostly compatible but superior to the well-known ElementTree API.


The .xpath() method
-------------------

http://lxml.de/xpathxslt.html#xpath

lxml.etree supports the simple path syntax of the ``.find()``, ``.findall()``,
``.iterfind()`` and ``.findtext()`` methods on ElementTree and Element, as
known from the original ElementTree library (ElementPath). As an lxml specific
extension, these classes also provide an ``.xpath()`` method that supports
expressions in the **complete XPath syntax**, as well as custom extension
functions.

For ElementTree, the ``.xpath()`` method performs a global XPath query against
the document (if absolute) or against the root node (if relative).

When ``.xpath()`` is used on an Element, the XPath expression is evaluated against
the element (if relative) or against the root tree (if absolute).



===============================================================================
ElementTree library
===============================================================================

- http://effbot.org/zone/element-xpath.htm
- https://docs.python.org/3.6/library/xml.etree.elementtree.html#reference
- https://docs.python.org/3.6/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element

The ElementTree library comes with a simple XPath-like path language
called ElementPath. The main difference is that you can use the
{namespace}tag notation in ElementPath expressions. However, advanced
features like value comparison and functions are not available.

In addition to a full XPath implementation, lxml.etree supports the
ElementPath language in the same way ElementTree does, even using
(almost) the same implementation. The API provides four methods here
that you can find on Elements and ElementTrees:

.iterfind()
    Iterates over all Elements that match the path expression.

.findall()
    Returns a list of matching Elements.

.find()
    Efficiently returns only the first match.

.findtext()
    Returns the ``.text`` content of the first match.


XPath Support in ElementTree
----------------------------

ElementTree provides limited support for XPath expressions. The goal is to
support a small subset of the abbreviated syntax; a full XPath engine is
outside the scope of the core library.

In its simplest form, a location path is one or more tag names, separated by
slashes (``/``). You can also use an asterisk (``*``) instead of a tag name, to
match all elements at that level. For example, ``*/subtag`` returns all subtag
grandchildren.

An empty tag (``//``) is used to search on all levels of the tree, beneath the
current level. The empty tag must always be followed by a tag name or an
asterisk. For example, ``.//tag`` returns all tag elements in the entire tree.

When searching on individual elements, the path must not start with a slash.
You can add a leading period (``.``), if necessary.

Path element summary:

``tag``
    Selects all child elements with the given tag. For example, ``spam``
    selects all child elements named “spam”, ``spam/egg`` selects all
    grandchildren named “egg” in all child elements named “spam”. You can use
    universal names (``{url}local``) as tags.

``*``
    Selects all child elements. For example, ``*/egg`` selects all
    grandchildren named “egg”.

``.``
    Select the current node. This is mostly useful at the beginning of a path,
    to indicate that it’s a relative path.

``//``
    Selects all subelements, on all levels beneath the current element (search
    the entire subtree). For example, ``.//egg`` selects all “egg” elements in
    the entire tree.

``..``
    Selects the parent element.

``[@attrib]``
    Selects all elements that have the given attribute. For example,
    ``.//a[@href]`` selects all “a” elements in the tree that has a “href”
    attribute.

``[@attrib=’value’]``
    Selects all elements for which the given attribute has the given value. For
    example, ``.//div[@class=’sidebar’]`` selects all “div” elements in the
    tree that has the class “sidebar”. In the current release, the value cannot
    contain quotes.

``[tag]``
    Selects all elements that has a child element named tag. In the current
    version, only a single tag can be used (i.e. only immediate children are
    supported).

``[position]``
    Selects all elements that are located at the given position. The position
    can be either an integer (1 is the first position), the expression
    ``last()`` (for the last position), or a position relative to last() (e.g.
    ``last()-1`` for the second to last position). This predicate must be
    preceeded by a tag name.

Note that ``find`` and ``findtext`` still returns information about the first
matching element only (first in document order, that is). To get more than one
element, use the findall method.
