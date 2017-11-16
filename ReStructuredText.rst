###############################################################################
reStructuredText and Sphinx syntax overview
###############################################################################

- http://docutils.sourceforge.net/docs/user/rst/quickref.html
- http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html
- http://www.sphinx-doc.org/en/stable/rest.html


Contents:

- `Escaping Mechanism`_
- `Reference Names`_
- `Inline markup`_
- `Sections and Titles`_
- `Paragraphs`_
- `Lists`_
    - `Bullet Lists`_
    - `Enumerated lists`_
    - `Definition Lists`_
    - `Field Lists`_
    - `Option Lists`_
- `Hyperlinks`_
    - `Standalone Hyperlinks`_
    - `Embedded URIs and Aliases`_
    - `External hyperlink targets`_
    - `Implicit hyperlink targets`_
    - `Explicit hyperlink targets`_
    - `Anonymous Hyperlinks`_
- `Literal Blocks`_
- `Line Blocks`_
- `Block Quotes`_
- `Doctest Blocks`_
- `Tables`_
- `Substitution Definitions`_
- `Comments`_
- `Footnotes`_
- `Citations`_
- `Transitions`_
- `Units`_
- `Directives`_
    - `Image directive`_
    - `Figure directive`_
    - `Code directive`_
    - `Sphinx code-block directive`_
    - `Include directive`_
    - `Sphinx literalinclude directive`_
    - `Topic directive`_
    - `Specific Admonitions directives`_
    - `Generic Admonition directive`_
    - `Unicode Character Codes directive`_



===============================================================================
Escaping Mechanism
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#escaping-mechanism

A backslash (``\``) followed by any character (except whitespace characters in
non-URI contexts) escapes that character. The escaped character represents the
character itself, and is prevented from playing a role in any markup
interpretation.

A literal backslash is represented by two backslashes in a row (``\\``).

In non-URI contexts, backslash-escaped whitespace characters are removed from
the document. This allows for character-level inline markup.

In URIs, backslash-escaped whitespace represents a single space.

There are two contexts in which backslashes have no special meaning: literal
blocks and inline literals. In these contexts, a single backslash represents a
literal backslash, without having to double up.



===============================================================================
Reference Names
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#reference-names

Footnote labels, citation labels, interpreted text roles, and some hyperlink
references use the simple reference name syntax.

Reference names are **whitespace-neutral** and **case-insensitive**. When
resolving reference names internally:

- whitespace is normalized (one or more spaces, horizontal or vertical tabs,
  newlines, carriage returns are interpreted as a single space)
- case is normalized (all alphabetic characters are converted to lowercase)



===============================================================================
Inline markup
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#inline-markup

::

    *emphasis*
    **strong emphasis**
    `interpreted text`
    ``inline literal``
    reference_
    `phrase reference`_
    anonymous__
    _`inline internal target`
    |substitution reference|
    footnote reference [1]_
    citation reference [CIT2002]_
    http://a.standalone/hyperlink/

Inline markup applies to words or phrases within a text block.

- The text within inline markup may not begin or end with whitespace.
- Inline markup cannot be nested.
- It must be separated from surrounding text by non-word characters
  like a space.

The inline markup recognition order is as follows:

- Asterisks: Strong emphasis (**) is recognized before emphasis (*).
- Backquotes: Inline literals, inline internal targets, are mutually
  independent, and are recognized before phrase hyperlink references and
  interpreted text.
- Trailing underscores: Footnote references ("[" + label + "]_") and simple
  hyperlink references (name + trailing "_") are mutually independent.
- Vertical bars: Substitution references ("|") are independently recognized.
- Standalone hyperlinks are the last to be recognized.

The following terms do require either **literal-quoting** or **escaping** to
avoid misinterpretation::

    *4, class_, *args, **kwargs, `TeX-quoted', *ML, *.txt

In most use cases, inline literals or literal blocks are the best choice.
Alternatively, the inline markup characters can be escaped::

    \*4, class\_, \*args, \**kwargs, \`TeX-quoted', \*ML, \*.txt

Backslash workaround:

- ``*longish*`` is correct and gives: *longish*.
- ``long*ish*`` is not interpreted as expected: long*ish*.
- We should use ``long\ *ish*`` to obtain: long\ *ish*.

The double backquote (``) is used to enter in verbatim mode, which can be used
as the escaping character.

In Python docstrings it will be necessary to escape any backslash characters so
that they actually reach reStructuredText. The simplest way to do this is to
use raw strings by adding the letter "r" in front of the docstring:

====================================  =========================
Python string                   	  Typical result
====================================  =========================
``r"""\*escape* \`with` "\\""""``     ``*escape* `with` "\"``
``"""\\*escape* \\`with` "\\\\""""``  ``*escape* `with` "\"``
``"""\*escape* \`with` "\\""""``      ``escape with ""``
====================================  =========================



===============================================================================
Sections and Titles
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#sections

Sections are identified through their titles, which are marked up with
"underlines" below the title text. Or under and overlines are used, their
length must be identical.

Underline-only adornment styles are distinct from overline-and-underline styles
that use the same character.

The following characters are recommended::

    = - ` : . ' " ~ ^ _ * + #

There are no heading levels assigned to certain characters as the structure is
determined from the succession of headings::

    ##########################
    Title
    ##########################

    Subtitle
    =========================

    SubSubTitle
    -----------

A blank line after a title is optional. All text blocks up to the next title of
the same or higher level are included in a section (or subsection, etc.).

Each section title automatically generates a hyperlink target pointing to the
section. The text of the hyperlink target (the "reference name") is the same as
that of the section title.



===============================================================================
Paragraphs
===============================================================================

Paragraph is a chunk of text that is separated by blank lines (one is enough).
Paragraphs must have the same indentation. Paragraphs that start indented will
result in indented quote paragraphs::

    This is a paragraph. It's quite
    short.

        This paragraph will result in an indented block of
        text, typically used for quoting other text.

    This is another one.



===============================================================================
Lists
===============================================================================

In all list cases, you may have as many paragraphs, sublists, etc. as you want,
as long as the left-hand side of the paragraph or whatever aligns with the
first line of text in the list item.

Lists must always start a new paragraph, they must appear after a blank line.


Bullet Lists
------------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#bullet-lists

A text block which begins with a **-** , **\***, **+** and others followed by
whitespace, is a bullet list item ("unordered" list item). List item bodies
must be left-aligned and indented relative to the bullet; the text immediately
after the bullet determines the indentation::

    - This is the first bullet list item.  The blank line above the
      first list item is required; blank lines between list items
      (such as below this paragraph) are optional.

    - This is the first paragraph in the second item in the list.

      This is the second paragraph in the second item in the list.
      The blank line above this paragraph is required.  The left edge
      of this paragraph lines up with the paragraph above, both
      indented relative to the bullet.

        - This is a sublist.  The bullet lines up with the left edge of
          the text blocks above.  A sublist is a new list so requires a
          blank line above and below.

    - This is the third item of the main list.

    This paragraph is not part of the list.


Enumerated lists
----------------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#enumerated-lists

The following enumeration sequences are recognized:

- arabic numerals: 1, 2, 3, ... (no upper limit).
- uppercase alphabet characters: A, B, C, ..., Z.
- lower-case alphabet characters: a, b, c, ..., z.
- uppercase Roman numerals: I, II, III, IV, ..., MMMMCMXCIX (4999).
- lowercase Roman numerals: i, ii, iii, iv, ..., mmmmcmxcix (4999).

The auto-enumerator (``#``) can be used to automatically enumerate a
list. Auto-enumerated lists may begin with explicit enumeration, which sets the
sequence. Fully auto-enumerated lists use arabic numerals and begin with 1.

The following formatting types are recognized:

- suffixed with a period: "1.", "A.", "a.", "I.", "i.".
- surrounded by parentheses: "(1)", "(A)", "(a)", "(I)", "(i)".
- suffixed with a right-parenthesis: "1)", "A)", "a)", "I)", "i)".

::

    1. Item 1 text

        a) Item 1a
        b) Item 1b

    2. Item 2 text

        a) Item 2a
        b) Item 2b

    #. This item is auto-enumerated


Definition Lists
----------------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#definition-lists

Each definition list item contains a term, optional classifiers, and a
definition.

A term is a simple one-line word or phrase. Optional classifiers may follow the
term on the same line, each after an inline " : " (note spaces). A definition
is a block indented relative to the term, and may contain multiple paragraphs
and other body elements.

There may be no blank line between a term line and a definition block (this
distinguishes definition lists from block quotes). Blank lines are required
before the first and after the last definition list item, but are optional
in-between::

    term 1
        Definition 1

    term 2
        Definition 2, paragraph 1

        Definition 2, paragraph 2

    term 3 : classifier
        Definition 3

    term 4 : classifier one : classifier two
        Definition 4

Inline markup is parsed in the term line before the classifier delimiter
(" : ") is recognized. The delimiter will only be recognized if it appears
outside of any inline markup.


Field Lists
-----------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#field-lists

Field lists are used as part of an extension syntax, such as options for
directives. They may also be used for key-value table-like structures::

    :Date: 2001-08-16
    :Version: 1.23
    :Authors: - John
              - Michael
              - Peter
    :Indentation: Since the field marker may be quite long, the second
        and subsequent lines of the field body do not have to line up
        with the first line, but they must be indented relative to the
        field name marker, and they must line up with each other.
    :Parameter i: integer


Option Lists
------------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#option-lists

Option lists are two-column lists of command-line options and descriptions,
documenting a program's options::

    -a         Output all.
    -b         Output both (this description is
               quite long).
    -c arg     Output just arg.
    --long     Output all day long.
    -p         This option has two paragraphs in the description.
               This is the first.

               This is the second.  Blank lines may be omitted between
               options (as above) or left in (as here and below).

    --very-long-option  A VMS-style option. Note the adjustment for
                        the required two spaces.

    --an-even-longer-option
            The description can also start on the next line.

    -2, --two  This option has two variants.

    -f FILE, --file=FILE  These two options are synonyms; both have
                          arguments.

    /V         A VMS/DOS-style option.



===============================================================================
Hyperlinks
===============================================================================

Standalone Hyperlinks
---------------------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#standalone-hyperlinks

An absolute URI [16] or standalone email address within a text block is treated
as a general external hyperlink with the URI itself as the link's text::

    http://www.example.org/
    john@example.org

Punctuation at the end of a URI is not considered part of the URI, unless the
URI is terminated by a closing angle bracket (">"). Backslashes may be used in
URIs to escape markup characters, specifically asterisks and underscores which
are vaid URI characters.


Embedded URIs and Aliases
-------------------------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#embedded-uris-and-aliases

A hyperlink reference may directly embed a target URI or a hyperlink
reference::

    See the `Python home page <http://www.python.org>`_ for info.

    This `link <Python home page_>`_ is an alias to the link above.

The bracketed URI must be preceded by whitespace and be the last text before
the end string.


External hyperlink targets
--------------------------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#hyperlink-targets

External hyperlink targets have an absolute or relative URI or email address in
their link blocks::

    See the Python_ home page for info.

    `Write to me`_ with your questions.

    .. _Python: http://www.python.org
    .. _Write to me: jdoe@example.com

    This is a paragraph that contains `some link`_.

    .. _some link: http://example.com/

If you have an underscore within the label/name, you got to escape it
with a backslash (``\``) character.

If an external hyperlink target's URI contains an underscore as its last
character, it must be escaped to avoid being mistaken for an indirect hyperlink
target::

    This link_ refers to a file called ``underscore_``.

    .. _link: underscore\_


Implicit hyperlink targets
--------------------------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#implicit-hyperlink-targets

Implicit hyperlink targets are generated by section titles, footnotes, and
citations, and may also be generated by extension constructs.

All titles are considered as hyperlinks. A link to a title is just its name
within quotes and a final underscore::

    `Some Title`_

This syntax works only if the title and link are within the same RST file. If
this is not the case, then you need to create a label before the title and
refer to this new link explicitly, as explained in Explicit Links section.


Explicit hyperlink targets
--------------------------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#hyperlink-targets

Hyperlink targets identify a location within or outside of a document, which
may be linked to by hyperlink references. Hyperlink targets may be **named** or
**anonymous**. Reference names are whitespace-neutral and case-insensitive.

Create a label "some_label"::

    .. _some_label:

Refer to this label, first method::

    some_label_

The second method use the ``ref`` role::

    :ref:`some_label`

With the first method, the link appears as "some_label", whereas the second
method use the first title’s name found after the label.

- We need use the ``ref`` role if the link is to be found in an external file.
- If we use the ``ref`` role, the final underscore is not required anymore.


Anonymous Hyperlinks
--------------------

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#anonymous-hyperlinks

Anonymous hyperlinks are designed to allow convenient verbose hyperlink
references.

Anonymous hyperlink references are specified with two underscores
instead of one::

    See `web site`__.

Anonymous targets, no reference name is required or allowed::

    .. __: http://example.org

As alternative, anonymous targets may begin with two underscores only::

    __ http://example.org

The order of anonymous hyperlink references and targets within the document is
significant: the first anonymous reference will link to the first anonymous
target.

The number of anonymous hyperlink references in a document must match
the number of anonymous targets.



===============================================================================
Literal Blocks
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#literal-blocks

A paragraph consisting of two colons ("::") signifies that the following text
blocks comprise a literal block. The literal block must either be **indented**
or **quoted**. No markup processing is done within a literal block.

Blank lines are required before and after a literal block, but these blank
lines are not included as part of the literal block.::

    This is a typical paragraph. An indented literal block follows.

    ::

        for a in [5,4,3,2,1]:   # this is program code, shown as-is
            print a
        print "Hello"
        # a literal block continues until the indentation ends

    This text has returned to the indentation of the first paragraph,
    is outside of the literal block, and is therefore treated as an
    ordinary paragraph.

As a convenience, the "::" is recognized at the end of any paragraph.

Expanded form::

    Paragraph:

    ::

        Literal block

Partially minimized form::

    Paragraph: ::

        Literal block

Fully minimized form::

    Paragraph::

        Literal block

**Quoted literal blocks** are unindented contiguous blocks of text where each
line begins with the same non-alphanumeric printable 7-bit ASCII character.
A blank line ends a quoted literal block::

    John Doe wrote::

    > Useful for quotes from email and
    > for Haskell literate programming.


The following are all valid quoting characters::

    ! " # $ % & ' ( ) * + , - . / : ; < = > ? @ [ \ ] ^ _ ` { | } ~

The quoting characters are preserved in the processed document.



===============================================================================
Line Blocks
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#line-blocks

Line blocks are useful for text, where the structure of lines is significant.
Line blocks are groups of lines beginning with vertical bar ("|") prefixes.

Each vertical bar prefix indicates a new line, so line breaks are preserved.
Initial indents are also significant, resulting in a nested structure.

Continuation lines are wrapped portions of long lines; they begin with a space
in place of the vertical bar. The left edge of a continuation line must be
indented. A line block ends with a blank line.

::

    | Line blocks are useful for addresses,
    | verse, and adornment-free lists.
    | 
    | Continuation lines are wrapped portions of long lines; they begin
      with spaces in place of vertical bars.
    | Inline markup **is supported**.
    |     Line breaks and initial indents
    |     are preserved.



===============================================================================
Block Quotes
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#block-quotes

A text block that is indented relative to the preceding text, without preceding
markup indicating it to be a literal block or other content, is a block quote.
All markup processing (for body elements and inline markup) continues within
the block quote.

A block quote may end with an attribution: a text block beginning with "--",
"---", flush left within the block quote. If the attribution consists of
multiple lines, the left edges of the second and subsequent lines must align::

    This is an ordinary paragraph, introducing a block quote.

        "It is **my business** to know things. That is **my trade**."

        -- Sherlock Holmes

Blank lines are required before and after a block quote, but these blank lines
are not included as part of the block quote.

Empty comments may be used to explicitly terminate preceding constructs that
would otherwise consume a block quote::

    * List item.

    ..

        Block quote

Empty comments may also be used to separate block quotes::

        Block quote one

    ..

        Block quote another



===============================================================================
Doctest Blocks
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#doctest-blocks

The doctest module searches a module's docstrings for text that looks like an
interactive Python session, then executes all such sessions to verify they
still work exactly as shown.



===============================================================================
Tables
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#tables

The rendering of the table depends on the CSS/HTML style, not on sphinx itself.

Simple tables
-------------

Simple tables provide a compact and easy to type but limited row-oriented table
representation for simple data sets. Cell contents are typically single
paragraphs, although arbitrary body elements may be represented in most cells.
Simple tables allow multi-line rows (in all but the first column) and column
spans, but not row spans.

Cells in the first column of new rows (not continuation lines) must contain
some text; blank cells would lead to a misinterpretation. Also, this mechanism
limits cells in the first column to only one line of text.

::

    =====  =====  ======
       Inputs     Output
    ------------  ------
      A      B    A or B
    =====  =====  ======
    False  False  False
    True   False  True
    =====  =====  ======


To start a new row in a simple table without text in the first column in the
processed output:

- An empty comment (".."), which may be omitted from the processed output.
- A backslash escape ("\") followed by a space.

The rightmost column is unbounded; text may continue past the edge of the
table. However, it is recommended that borders be made long enough to contain
the entire text.::

    =====  =====
    col 1  col 2
    =====  =====
    1      Second column of row 1.
    2      Second column of row 2.
           Second line of paragraph.
    3      - Second column of row 3.

           - Second item in bullet
             list (row 3, column 2).
    =====  =====


Grid tables
-----------

::

    +------------+------------+-----------+
    | Header 1   | Header 2   | Header 3  |
    +============+============+===========+
    | body row 1 | column 2   | column 3  |
    +------------+------------+-----------+
    | body row 2 | Cells may span columns.|
    +------------+------------+-----------+
    | body row 3 | Cells may  | - Cells   |
    +------------+ span rows. | - contain |
    | body row 4 |            | - blocks. |
    +------------+------------+-----------+


CSV-table directive
-------------------

http://docutils.sourceforge.net/docs/ref/rst/directives.html#id4

Create a table from CSV (comma-separated values) data. The data may be internal
(an integral part of the document) or external (a separate file)::

    .. csv-table:: a title
        :header: "name", "firstname", "age"
        :widths: 20, 20, 10

        "Smith", "John", 40
        "Smith", "John, Junior", 20



===============================================================================
Substitution Definitions
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#substitution-definitions

Substitution references are replaced in-line by the processed contents of the
corresponding definition (linked by matching substitution text). Matches are
case-sensitive but forgiving; if no exact match is found, a case-insensitive
comparison is attempted.

They are a way to include arbitrarily complex inline structures within text,
while keeping the details out of the flow of text.

::

    .. |some text| replace:: Some very long text

    .. |some icon| image:: icon.png
        :width: 20pt
        :height: 20pt

    Some |some text| with |some icon| here!



===============================================================================
Comments
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#comments

The only restriction on comments is that they not use the same syntax as any of
the other explicit markup constructs: substitution definitions, directives,
footnotes, citations, or hyperlink targets. To ensure that none of the other
explicit markup constructs is recognized, leave the ".." on a line by itself::

    ..  This is a comment.
        This text will not be shown
        (but, for instance, in HTML might be rendered as an HTML comment)

    ..
        And this is comment too.

An **empty comment** is ".." with blank lines before and after. An empty
comment does not consume following blocks::

    ..

        So this block is not comment,
        despite its indentation.



===============================================================================
Footnotes
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#footnotes

For footnotes, use ``[#name]_`` to mark the footnote location, and add the
footnote body at the bottom of the document after a “Footnotes” rubric
heading::

    Some text that requires a footnote [#f1]_ .

    .. rubric:: Footnotes

    .. [#f1] Text of the first footnote.

We can also explicitly number the footnotes (``[1]_``) or use auto-numbered
footnotes without names (``[#]_``).



===============================================================================
Citations
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#citations

Citation labels are simple reference names (case-insensitive single words
consisting of alphanumerics plus internal hyphens, underscores, and periods; no
whitespace)::

    .. [CIT2002] This is the citation. It's just like a footnote,
        except the label is textual.

And then called anywhere::

    Here is a citation reference: [CIT2002]_



===============================================================================
Transitions
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#transitions

Transitions separate other body elements. A transition should not begin or end
a section or document, nor should two transitions be immediately adjacent.

The syntax for a transition marker is a horizontal line of 4 or more repeated
punctuation characters. The syntax is the same as section title underlines
without title text. Transition markers require blank lines before and after::

    Para.

    ----------

    Para.



===============================================================================
Units
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#units

All measures consist of a positive floating point number in standard
(non-scientific) notation and a unit, possibly separated by one or more spaces.

Units are only supported where explicitly mentioned in the reference manuals.

The following **length units** are supported by the reStructuredText parser:

- px (pixels, relative to the canvas resolution)
- em (ems, the height of the element's font)
- ex (x-height, the height of the letter "x")
- in (inches; 1in=2.54cm)
- cm (centimeters; 1cm=10mm)
- mm (millimeters)
- pt (points; 1pt=1/72in)
- pc (picas; 1pc=12pt)

This set corresponds to the length units in CSS.

**Percentage units** have a percent sign ("%") as unit. Percentage values are
relative to other values, depending on the context in which they occur.



===============================================================================
Directives
===============================================================================

http://docutils.sourceforge.net/docs/ref/rst/directives.html

Directives are an extension mechanism for reStructuredText, a way of adding
support for new constructs without adding new primary syntax (directives may
support additional syntax locally).


Image directive
---------------

http://docutils.sourceforge.net/docs/ref/rst/directives.html#images

There are two image directives: **image** and **figure**.

An "image" is a simple picture::

    .. image:: photo.png

Inline images can be defined with an "image" directive in a substitution
definition::

    The |biohazard| symbol must be used on containers.

    .. |biohazard| image:: biohazard.png

The image URI may begin on the same line as the explicit markup start and
target name, or it may begin in an indented text block immediately following,
with no intervening blank lines. If there are multiple lines in the link block,
they are stripped of leading and trailing whitespace and joined together.

Optionally, the image link block may contain an options::

    .. image:: cats.jpeg
        :width: 200px
        :height: 100px
        :scale: 50 %
        :alt: alternate text
        :align: right
        :target: http://example.org

The **target** makes the image into a hyperlink reference ("clickable").
The option argument may be a URI (relative or absolute), or a reference name
with underscore suffix (```some ref name`_``).


Figure directive
----------------

http://docutils.sourceforge.net/docs/ref/rst/directives.html#figure

A "figure" consists of image data (including image options), an optional
caption (a single paragraph), an optional legend (arbitrary body elements)::

    .. figure:: picture.png
        :scale: 50 %
        :alt: some alt text

        This is the caption of the figure (a simple paragraph).

        The legend consists of all elements after the caption. In this example,
        the legend consists of this paragraph and the following table:

        +-----------------------+-----------------------+
        | Symbol                | Meaning               |
        +=======================+=======================+
        | .. image:: waves.png  | Lake                  |
        +-----------------------+-----------------------+
        | .. image:: peak.png   | Mountain              |
        +-----------------------+-----------------------+

        There must be blank lines before the caption paragraph and before the
        legend. To specify a legend without a caption, use an empty comment
        ("..") in place of the caption.


Code directive
--------------

http://docutils.sourceforge.net/docs/ref/rst/directives.html#code
http://pygments.org/docs/lexers/#pygments.lexers.scripting.RexxLexer

The "code" directive constructs a literal block. If the code language is
specified, the content is parsed by the Pygments syntax highlighter::

    .. code:: python
        :number-lines:

        def my_function():
            "just a test"
            print 8/2

Not supported by Sphinx right now :( Wait for future updates.


Sphinx code-block directive
---------------------------

http://www.sphinx-doc.org/en/stable/markup/code.html
http://pygments.org/docs/lexers/#pygments.lexers.scripting.RexxLexer

Syntax highlighting is done with Pygments. Per default, this is 'python'. The
global highlighting language can be changed using the "highlight" directive::

    .. highlight:: language

We can directly specify the language using the "code-block" directive
(or alias "sourcecode")::

    .. sourcecode:: typescript
        :linenos:

        function add(left: number, right: number): number {
            return left + right;
        }

    .. code-block:: go
        :linenos:

        package main

        import "fmt"

        func main() {
            fmt.Println("Hello, 世界")
        }

The valid values for the highlighting language are:

* none - no highlighting.
* python - the default when highlight_language isn’t set.
* guess - let Pygments guess the lexer based on contents, only works
  with certain well-recognizable languages.
* Any other `lexer alias`_ that Pygments supports.

.. _lexer alias: http://pygments.org/docs/lexers/#pygments.lexers.scripting.RexxLexer


Include directive
-----------------

http://docutils.sourceforge.net/docs/ref/rst/directives.html#including-an-external-document-fragment

The "include" directive reads a text file. The directive argument is the path
to the file to be included, relative to the document containing the directive.
Unless the options literal or code are given, the file is parsed in the current
document's context at the point of the directive::

    This first example will be parsed at the document level, and can
    thus contain any construct, including section headers.

    .. include:: inclusion.txt

    Back in the main document.

        This second example will be parsed in a block quote context.
        Therefore it may only contain body elements.  It may not
        contain section headers.

        .. include:: inclusion.txt


Sphinx literalinclude directive
-------------------------------

http://www.sphinx-doc.org/en/stable/markup/code.html#includes

Long text may be included by storing the example text in an external file
containing only plain text::

    .. literalinclude:: file.py
        :linenos:
        :language: python
        :lines: 1, 3-5
        :start-after: 3
        :end-before: 5


Topic directive
---------------

http://docutils.sourceforge.net/docs/ref/rst/directives.html#topic

A topic is like a block quote with a title, or a self-contained section with no
subsections. Use it to indicate a self-contained idea that is separate from the
flow of the document. Topics may occur anywhere a section or transition may
occur. Body elements and topics may not contain nested topics.

The directive's sole argument is interpreted as the topic title; the next line
must be blank. All subsequent lines make up the topic body, interpreted as body
elements::

    .. topic:: Some Topic Title

        Subsequent indented lines comprise
        the body of the topic, and are
        interpreted as body elements.



Specific Admonitions directives
-------------------------------

http://docutils.sourceforge.net/docs/ref/rst/directives.html#specific-admonitions

Any text immediately following the directive indicator (on the same line and/or
indented on following lines) is interpreted as a directive block and is parsed
for normal body elements.

Admonitions are specially marked "topics" that can appear anywhere an ordinary
body element can. They contain arbitrary body elements. Typically, an
admonition is rendered as an offset block in a document, sometimes outlined or
shaded, with a title matching the admonition type.

The following admonition directives have been implemented::

    attention caution danger error hint important note tip warning

Any text immediately following the directive indicator (on the same line and/or
indented on following lines) is interpreted as a directive block and is parsed
for normal body elements::

    .. note:: This is a note admonition.
        This is the second line of the first paragraph.

        - The note contains all indented body elements
          following.
        - It includes this bullet list.

    .. WARNING::
        Beware killer rabbits!

    .. tip:: Don't worry, be happy!


Generic Admonition directive
----------------------------

http://docutils.sourceforge.net/docs/ref/rst/directives.html#generic-admonition

This is a generic, titled admonition. The title may be set to anything::

    .. admonition:: Here title!

        And, by the way...
        You can make up your own admonition.


Unicode Character Codes directive
---------------------------------

http://docutils.sourceforge.net/docs/ref/rst/directives.html#unicode-character-codes

The "unicode" directive converts Unicode character codes (numerical values) to
characters, and may be used in substitution definitions only.

The arguments, separated by spaces, can be:

- character codes as decimal numbers or hexadecimal numbers, prefixed by
  ``0x, x, \x, U+, u, \u``
  or as XML-style hexadecimal character entities, e.g. &#x1a2b;
- text, which is used as-is.

Hexadecimal codes are case-insensitive.

Text following " .. " is a comment and is ignored. The spaces between the
arguments are ignored and thus do not appear in the output::

    2003 |---| 2017 |(c)| SomeCorp |(TM)|

    .. |(c)| unicode:: 0xA9 .. copyright sign
    .. |(TM)| unicode:: U+2122 .. trademark sign
       :ltrim:
    .. |---| unicode:: U+02014 .. em dash
       :trim:

Results in:

    2003—2017 © SomeCorp™

The options ``ltrim/rtrim/trim`` are recognized: whitespace on specific
side or sides of the substitution reference is removed.

