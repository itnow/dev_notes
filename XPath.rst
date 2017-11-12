###############################################################################
XPath (XML Path Language)
###############################################################################

It uses a non-XML syntax to provide a flexible way of addressing (pointing to)
different parts of an XML document. It can also be used to test addressed nodes
within a document to determine whether they match a pattern or not.

- https://www.w3.org/TR/xpath-31/
- https://msdn.microsoft.com/en-us/library/ms256471(v=vs.110).aspx
- https://developer.mozilla.org/en-US/docs/Web/XPath
- https://www.w3schools.com/xml/xpath_syntax.asp



Location Path Expression
===============================================================================

XPath uses **path expressions** to select nodes in an XML document. The node is
selected by following a path or steps.

An absolute location path starts with a slash (``/``) and a relative location
path does not. In both cases the location path consists of one or more
**steps**, each separated by a slash.

An **absolute location path**::

    /step/step/step/...

A **relative location path**::

    step/step/step/...

Each step is evaluated against the nodes in the current node-set.

**Location step** syntax::
    
    axis::nodetest[predicate]

axis
    *Is optional*. Specifies the tree relationship between the context node and
    the nodes to be selected by the location step. In other words, the axis
    indicates the general direction that the location step proceeds from the
    context node. In a location step. If omitted, the axis defaults to
    ``child::``. Furthermore, several axes have shortcut forms; for instance,
    the ampersand (``@``) character is a shortcut for the attribute axis.

nodetest
    Specifies the node type or expanded name of the nodes to be initially
    selected by the location step. Basically, the node test indicates which
    node(s), from among all nodes on the indicated axis, to consider as
    candidates, that is, potential, matches for the location step.

predicate
    *Is optional*. Uses an XPath expression (condition to be met) to further
    refine the set of nodes selected by the location step. The predicate is a
    filter, specifying a selection criterion for further refining the list of
    candidate nodes.


Abbreviated/Unabbreviated location path
---------------------------------------

In an abbreviated location path, the axis specifier, ``axis-name::``, is not
expressed explicitly in a location step, but implied by a set of shortcuts
instead.

===============================  ============
Unabbreviated	                 Abbreviated
===============================  ============
``child::*``                     ``*``
``attribute::*``                 ``@*``
``/descendant-or-self::node()``  ``//``
``self::node()``                 ``.``
``parent::node()``               ``..``
===============================  ============



Selecting Nodes
===============================================================================

The node-set selected by a location step results from generating an initial
node-set based on the relationship between the axis and node test, and then
filtering that initial node-set by each of the predicates in turn.

The initial node-set consists of those nodes that meet the following
two criteria:

- The nodes have the relationship to the context node specified by the axis.
- The nodes have the node type and expanded name specified by the node test.

XPath then uses the first predicate in the location step to filter the initial
node-set to generate a new node-set. XPath then uses the second predicate to
filter the node-set resulting from the first predicate. This filtering process
repeats until XPath has evaluated all the predicates. The node-set that results
after applying all the predicates is the node-set selected by the location
step.

*Note:* Because the axis affects the evaluation of the expression in each
predicate, the semantics of a predicate is defined with respect to the
specified axis.


================  =============================================================
``nodetest``      Selects all nodes with the name "nodetest".
``/``             Selects from the root node.
``//``            Selects nodes in the document from the current node
                  that match the selection no matter where they are.
``.``             Indicates the current context. Selects the current node.
``..``            Selects the parent of the current context node.
``*``             Matches any element node.
``@``             Prefix for an attribute name. Selects attributes.
``@*``            Attribute wildcard. Selects all attributes regardless
                  of name.
``node()``        Matches any node of any kind.
``/bookstore/*``  Selects all the child element nodes of the bookstore element.
``//*``           Selects all elements in the document.
``//title[@*]``   Selects all title elements which have at least one attribute
                  of any kind
================  =============================================================


**Examples:**

``bookstore``
    Selects all nodes with the name "bookstore".

``/bookstore``
    Selects the root element bookstore.
    Note: If the path starts with a slash "/" it always represents
    an **absolute** path to an element.

``bookstore/book``
    Selects all book elements that are children of bookstore

``//book``
    Selects all book elements no matter where they are 
    in the document

``bookstore//book``
    Selects all book elements that are descendant of the bookstore
    element, no matter where they are under the bookstore element

``//@lang``
    Selects all attributes that are named lang



Predicates
===============================================================================

Predicates are used to find a specific node or a node that contains
a specific value. Predicates are always embedded in square brackets.

A predicate is an XPath expression that filters a node-set with respect to an
axis and produces a new node-set. This filtering process involves sequentially
evaluating the predicate against each node in the node set. Each time that the
predicate is evaluated against a node:

- The context node is the node currently being evaluated.
- The context size is the number of nodes in the node-set being evaluated.
- The context position is the position of the context node in the node-set.

This last context, that of the context node in the node-set, is relative to the
direction that the axis specified in the location step navigates the document
tree. Typically, an axis navigates the tree in either a forward or reverse
direction:

- A forward axis is an axis that contains the context node or nodes that are
  after the context node. The ``child::``, ``descendant::``,
  ``descendant-or-self::``, ``following::``, and ``following-sibling::`` axes
  are forward axes. These forward axes number the nodes in the node-set in
  document order, starting with the first position as 1.

- A reverse axis is an axis that contains the context node or nodes that are
  before the context node. The ``ancestor::``, ``ancestor-or-self::``,
  ``preceding::``, and ``preceding-sibling::`` axes are reverse axes. These
  reverse axes number the nodes in the node-set in reverse document order,
  starting with the first position as 1.

As to the remaining axes, the ``self::`` and ``parent::`` axes return a single
node. Therefore, the designation of forward or reverse does not make sense for
these two axes. There is no order defined for the ``attribute::`` and
``namespaces::`` axes; so they too are neither forward nor reverse axes.


**Examples:**

``/bookstore/book[1]``
    Selects the first book element that is the child of the bookstore element.
    Note: In IE 5,6,7,8,9 first node is[0], but according to W3C, it is [1].
    To solve this problem in IE, set the SelectionLanguage to XPath:
    In JavaScript: xml.setProperty("SelectionLanguage","XPath");

``/bookstore/book[last()]``
    Selects the last book element that is the child of the bookstore element

``/bookstore/book[last()-1]``
    Selects the last but one book element that is the child
    of the bookstore element

``/bookstore/book[position()<3]``
    Selects the first two book elements that are children
    of the bookstore element

``//title[@lang]``
    Selects all the title elements that have an attribute named lang

``//title[@lang='en']``
    Selects all the title elements that have a "lang" attribute with
    a value of "en"

``/bookstore/book[price>35.00]``
    Selects all the book elements of the bookstore element that have
    a price element with a value greater than 35.00

``/bookstore/book[price>35.00]/title``
    Selects all the title elements of the book elements of the bookstore element
    that have a price element with a value greater than 35.00



Axes
===============================================================================

An axis represents a relationship to the context node, and is used to locate
nodes relative to that node on the tree.

+--------------------+--------------------------------------------------------+
| ancestor           | Selects all ancestors (parent, grandparent, etc.)      |
|                    | of the current node.                                   |
+--------------------+--------------------------------------------------------+
| ancestor-or-self   | Selects all ancestors (parent, grandparent, etc.)      |
|                    | of the current node and the current node itself.       |
+--------------------+--------------------------------------------------------+
| attribute          | Selects all attributes of the current node.            |
+--------------------+--------------------------------------------------------+
| child              | Selects all children of the current node.              |
+--------------------+--------------------------------------------------------+
| descendant         | Selects all descendants (children, grandchildren, etc.)|
|                    | of the current node.                                   |
+--------------------+--------------------------------------------------------+
| descendant-or-self | Selects all descendants (children, grandchildren, etc.)|
|                    | of the current node and the current node itself.       |
+--------------------+--------------------------------------------------------+
| following          | Selects everything in the document after the closing   |
|                    | tag of the current node.                               |
+--------------------+--------------------------------------------------------+
| following-sibling  | Selects all siblings after the current node.           |
+--------------------+--------------------------------------------------------+
| namespace          | Selects all namespace nodes of the current node.       |
+--------------------+--------------------------------------------------------+
| parent             | Selects the parent of the current node.                |
+--------------------+--------------------------------------------------------+
| preceding          | Selects all nodes that appear before the current node  |
|                    | in the document, except ancestors, attribute nodes     |
|                    | and namespace nodes.                                   |
+--------------------+--------------------------------------------------------+
| preceding-sibling  | Selects all siblings before the current node.          |
+--------------------+--------------------------------------------------------+
| self               | Selects the current node.                              |
+--------------------+--------------------------------------------------------+


**Examples:**

``child::book``
    Selects all book nodes that are children of the current node.

``attribute::lang``
    Selects the lang attribute of the current node.

``child::*``
    Selects all element children of the current node.

``attribute::*``
    Selects all attributes of the current node.

``child::text()``
    Selects all text node children of the current node.

``child::node()``
    Selects all children of the current node.

``descendant::book``
    Selects all book descendants of the current node.

``ancestor::book``
    Selects all book ancestors of the current node.

``ancestor-or-self::book``
    Selects all book ancestors of the current node - and the current as well
    if it is a book node.

``child::*/child::price``
    Selects all price grandchildren of the current node.



Operators
===============================================================================

=======  =========================  =========================
``|``    Computes two node-sets     //book | //cd
``+``    Addition                   6 + 4
``-``    Subtraction                6 - 4
``*``    Multiplication             6 * 4
``div``  Division                   8 div 4
``=``    Equal                      price=9.80
``!=``   Not equal                  price!=9.80
``<``    Less than                  price<9.80
``<=``   Less than or equal to      price<=9.80
``>``    Greater than               price>9.80
``>=``   Greater than or equal to   price>=9.80
``or``   or                         price=9.80 or price=9.70
``and``  and                        price>9.00 and price<9.90
``mod``  Modulus (div remainder)    5 mod 2
=======  =========================  =========================

**Examples:**

``//book/title | //book/price``
    Selects all the title AND price elements of all book elements.

``//title | //price``
    Selects all the title AND price elements in the document.

``/bookstore/book/title | //price``
    Selects all the title elements of the book element of the bookstore element
    AND all the price elements in the document.

``(book | magazine)/price``
    The node set containing all <price> elements of either <book> or <magazine>
    elements.



Comparsions
===============================================================================

To compare two objects in XPath, use the ``=`` sign to test for equality,
or use ``!=`` to test for inequality.

For a comparison operation, exactly two operands must be supplied. Comparisons
are then made by evaluating each operand, and converting them as needed, so
they are of the same type.

For information about ``&lt;`` and other binary comparison operators,
read the docs about "Binary Comparison Operators".

**Examples:**

``author[last-name = "Bob"]``
    All <author> elements that contain at least one <last-name> element with
    the value Bob.

``author[last-name[1] = "Bob"]``
    All <author> elements whose first <last-name> child element has the value
    Bob.

``author/degree[@from != "Harvard"]``
    All <author> elements that contain <degree> elements with a from attribute
    that is not equal to "Harvard".

``author[last-name = /editor/last-name]``
    All <author> elements that contain a <last-name> element that is the same
    as the <last-name> element inside the <editor> element under the root
    element.

``author[. = "Matthew Bob"]``
    All <author> elements whose string value is Matthew Bob.



XML Documents Examples
===============================================================================

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <bookstore>
        <book category="cooking">
            <title lang="en">Everyday Italian</title>
            <author>Giada De Laurentiis</author>
            <year>2005</year>
            <price>30.00</price>
        </book>
        <book category="children">
            <title lang="en">Harry Potter</title>
            <author>J K. Rowling</author>
            <year>2005</year>
            <price>29.99</price>
        </book>
        <book category="web">
            <title lang="en">XQuery Kick Start</title>
            <author>James McGovern</author>
            <author>Per Bothner</author>
            <author>Kurt Cagle</author>
            <author>James Linn</author>
            <author>Vaidyanathan Nagarajan</author>
            <year>2003</year>
            <price>49.99</price>
        </book>
        <book category="web">
            <title lang="en">Learning XML</title>
            <author>Erik T. Ray</author>
            <year>2003</year>
            <price>39.95</price>
        </book>
    </bookstore>


.. code-block:: xml

    <?xml version="1.0"?>
    <?xml-stylesheet type="text/xsl" href="myfile.xsl" ?>
    <bookstore specialty="novel">
        <book style="autobiography">
            <author>
                <first-name>Joe</first-name>
                <last-name>Bob</last-name>
                <award>Trenton Literary Review Honorable Mention</award>
            </author>
            <price>12</price>
        </book>
        <book style="textbook">
            <author>
                <first-name>Mary</first-name>
                <last-name>Bob</last-name>
                <publication>Selected Short Stories of
                    <first-name>Mary</first-name>
                    <last-name>Bob</last-name>
                </publication>
            </author>
            <editor>
                <first-name>Britney</first-name>
                <last-name>Bob</last-name>
            </editor>
            <price>55</price>
        </book>
        <magazine style="glossy" frequency="monthly">
            <price>2.50</price>
            <subscription price="24" per="year"/>
        </magazine>
        <book style="novel" id="myfave">
            <author>
                <first-name>Toni</first-name>
                <last-name>Bob</last-name>
                <degree from="Trenton U">B.A.</degree>
                <degree from="Harvard">Ph.D.</degree>
                <award>Pulitzer</award>
                <publication>Still in Trenton</publication>
                <publication>Trenton Forever</publication>
            </author>
            <price intl="Canada" exchange="0.7">6.50</price>
            <excerpt>
                <p>It was a dark and stormy night.</p>
                <p>But then all nights in Trenton seem dark and
                stormy to someone who has gone through what
                <emph>I</emph> have.</p>
                <definition-list>
                    <term>Trenton</term>
                    <definition>misery</definition>
                </definition-list>
            </excerpt>
        </book>
        <my:book xmlns:my="uri:mynamespace" style="leather" price="29.50">
            <my:title>Who's Who in Trenton</my:title>
            <my:author>Robert Bob</my:author>
        </my:book>
    </bookstore>

|

``child::node()``
    Select all the children of the context node, whatever their node type.

``attribute::name``
    Select the name attribute of the context node.

``attribute::*``
    Select all the attributes of the context node.

``descendant::para``
    Select the <para> element descendants of the context node.

``ancestor::div``
    Select all <div> ancestors of the context node.

``ancestor-or-self::div``
    Select the <div> ancestors of the context node and, if the context node is
    a <div> element, select the context node as well.

``descendant-or-self::para``
    Select the <para> element descendants of the context node and, if the
    context node is a <para> element, select the context node as well.

``self::para``
    Select the context node if it is a <para> element; otherwise select nothing.

``child::chapter/descendant::para``
    Select the <para> element descendants of the <chapter> element children of
    the context node.

``child::*/child::para``
    Select all <para> grandchildren of the context node.

``/``
    Select the document root (which is always the parent
    of the document element).

``/descendant::para``
    Select all the <para> elements in the same document as the context node.

``/descendant::olist/child::item``
    Select all the <item> elements that have an <olist> parent and that are
    in the same document as the context node.

``child::para[position()=1]``
    Select the first <para> child of the context node.

``child::para[position()=last()]``
    Select the last <para> child of the context node.

``child::para[position()=last()-1]``
    Select the next-to-last <para> child of the context node.

``child::para[position()&gt;1]``
    Select all the <para> children of the context node, except for the first
    <para> child of the context node.

``/descendant::figure[position()=42]``
    Select the forty-second <figure> element in the document.

``/child::doc/child::chapter[position()=5]/child::section[position()=2]``
    Select the second <section> element contained in the fifth <chapter>
    element of the <doc> document element.

``child::para[attribute::type="warning"]``
    Select all <para> children of the context node that have a typeattribute
    with the value "warning".

``child::para[attribute::type="warning"][position()=5]``
    Select the fifth <para> child of the context node that has a typeattribute
    with the value "warning".

``child::para[position()=5][attribute::type="warning"]``
    Select the fifth <para> child of the context node if that child has a
    typeattribute with the value "warning".

``child::chapter[child::title="Introduction"]``
    Select the <chapter> children of the context node that have one or more
    <title> children with string value equal to "Introduction".

``child::chapter[child::title]``
    Select the <chapter> children of the context node that have one or more
    <title> children.

``child::*[self::chapter or self::appendix]``
    Select the <chapter> and <appendix> children of the context node.

``child::*[self::chapter or self::appendix][position()=last()]``
    Select the last <chapter> or <appendix> child of the context node.

|

``./author``
    All <author> elements within the current context. Note that this is
    equivalent to the expression in the next row.

``author``
    All <author> elements within the current context.

``first.name``
    All <first.name> elements within the current context.

``/bookstore``
    The document element (<bookstore>) of this document.

``//author``
    All <author> elements in the document.

``book[/bookstore/@specialty=@style]``
    All <book> elements whose style attribute value is equal to the specialty
    attribute value of the <bookstore> element at the root of the document.

``author/first-name``
    All <first-name> elements that are children of an <author> element.

``bookstore//title``
    All <title> elements one or more levels deep in the <bookstore> element
    (arbitrary descendants). Note that this is different from the expression in
    the next row.

``bookstore/*/title``
    All <title> elements that are grandchildren of <bookstore> elements.

``bookstore//book/excerpt//emph``
    All <emph> elements anywhere inside <excerpt> children of <book> elements,
    anywhere inside the <bookstore> element.

``.//title``
    All <title> elements one or more levels deep in the current context. Note
    that this situation is essentially the only one in which the period
    notation is required.

``author/*``
    All elements that are the children of <author> elements.

``book/*/last-name``
    All <last-name> elements that are grandchildren of <book> elements.

``*/*``
    All grandchildren elements of the current context.

``*[@specialty]``
    All elements with the specialty attribute.

``@style``
    The style attribute of the current context.

``price/@exchange``
    The exchange attribute on <price> elements within the current context.

``price/@exchange/total``
    Returns an empty node set, because attributes do not contain element
    children. This expression is allowed by the XML Path Language (XPath)
    grammar, but is not strictly valid.

``book[@style]``
    All <book> elements with style attributes, of the current context.

``book/@style``
    The style attribute for all <book> elements of the current context.

``@*``
    All attributes of the current element context.

``./first-name``
    All <first-name> elements in the current context node. Note that this is
    equivalent to the expression in the next row.

``first-name``
    All <first-name> elements in the current context node.

``author[1]``
    The first <author> element in the current context node.

``author[first-name][3]``
    The third <author> element that has a <first-name> child.

``my:book``
    The <book> element from the my namespace.

``my:*``
    All elements from the my namespace.

``@my:*``
    All attributes from the my namespace (this does not include unqualified
    attributes on elements from the my namespace).

|

``book[last()]``
    The last <book> element of the current context node.

``book/author[last()]``
    The last <author> child of each <book> element of the current context node.

``(book/author)[last()]``
    The last <author> element from the entire set of <author> children of
    <book> elements of the current context node.

``book[excerpt]``
    All <book> elements that contain at least one <excerpt> element child.

``book[excerpt]/title``
    All <title> elements that are children of <book> elements that also contain
    at least one <excerpt> element child.

``book[excerpt]/author[degree]``
    All <author> elements that contain at least one <degree> element child, and
    that are children of <book> elements that also contain at least one
    <excerpt> element.

``book[author/degree]``
    All <book> elements that contain <author> children that in turn contain at
    least one <degree> child.

``author[degree][award]``
    All <author> elements that contain at least one <degree> element child and
    at least one <award> element child.

``author[degree and award]``
    All <author> elements that contain at least one <degree> element child and
    at least one <award> element child.

``author[(degree or award) and publication]``
    All <author> elements that contain at least one <degree> or <award> and at
    least one <publication> as the children

``author[degree and not(publication)]``
    All <author> elements that contain at least one <degree> element child and
    that contain no <publication> element children.

``author[not(degree or award) and publication]``
    All <author> elements that contain at least one <publication> element child
    and contain neither <degree> nor <award> element children.

``author[last-name = "Bob"]``
    All <author> elements that contain at least one <last-name> element child
    with the value Bob.

``author[last-name[1] = "Bob"]``
    All <author> elements where the first <last-name> child element has the
    value Bob. Note that this is equivalent to the expression in the next row.

``author[last-name [position()=1]= "Bob"]``
    All <author> elements where the first <last-name> child element has the
    value Bob.

``degree[@from != "Harvard"]``
    All <degree> elements where the from attribute is not equal to "Harvard".

``author[. = "Matthew Bob"]``
    All <author> elements whose value is Matthew Bob.

``author[last-name = "Bob" and ../price &gt; 50]``
    All <author> elements that contain a <last-name> child element whose value
    is Bob, and a <price> sibling element whose value is greater than 50.

``book[position() &lt;= 3]``
    The first three books (1, 2, 3).

``author[not(last-name = "Bob")]``
    All <author> elements that do no contain <last-name> child elements with
    the value Bob.

``author[first-name = "Bob"]``
    All <author> elements that have at least one <first-name> child with the
    value Bob.

``author[* = "Bob"]``
    all author elements containing any child element whose value is Bob.

``author[last-name = "Bob" and first-name = "Joe"]``
    All <author> elements that has a <last-name> child element with the value
    Bob and a <first-name> child element with the value Joe.

``price[@intl = "Canada"]``
    All <price> elements in the context node which have an intl attribute equal
    to "Canada".

``degree[position() &lt; 3]``
    The first two <degree> elements that are children of the context node.

``p/text()[2]``
    The second text node in each <p> element in the context node.

``ancestor::book[1]``
    The nearest <book> ancestor of the context node.

``ancestor::book[author][1]``
    The nearest <book> ancestor of the context node and this <book> element has
    an <author> element as its child.

``ancestor::author[parent::book][1]``
    The nearest <author> ancestor in the current context and this <author>
    element is a child of a <book> element.

|

``child::*[position()=1]``
    Locates the first child node of the context node.

``ancestor-or-self::book[@catdate="2000-12-31"]``
    Locates all ancestors of any <book> child of the context node, as well as
    the <book> child itself, as long as the element in question has a catdate
    attribute with the value "2000-12-31".

``//parent::node()[name()="book"] | descendant::node()[name()="author"]``
    Locates any node in the document whose parent node is named "book", or any
    node descended from the context node whose name is "author".
